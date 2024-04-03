## Overview

To test for [injection flaws](https://mas.owasp.org/MASTG/General/0x04h-Testing-Code-Quality#injection-flaws "Injection Flaws" "Injection Flaws") you need to first rely on other tests and check for functionality that might have been exposed:

- [Testing Deep Links](https://mas.owasp.org/MASTG/tests/android/MASVS-CODE/MASTG-TEST-0025/)
- [Testing for Sensitive Functionality Exposure Through IPC](https://mas.owasp.org/MASTG/tests/android/MASVS-CODE/MASTG-TEST-0025/)
- [Testing for Overlay Attacks](https://mas.owasp.org/MASTG/tests/android/MASVS-CODE/MASTG-TEST-0025/)

## Static Analysis

An example of a vulnerable IPC mechanism is shown below.

You can use _ContentProviders_ to access database information, and you can probe services to see if they return data. If data is not validated properly, the content provider may be prone to SQL injection while other apps are interacting with it. See the following vulnerable implementation of a _ContentProvider_.

`<provider     android:name=".OMTG_CODING_003_SQL_Injection_Content_Provider_Implementation"     android:authorities="sg.vp.owasp_mobile.provider.College"> </provider>`

The `AndroidManifest.xml` above defines a content provider that's exported and therefore available to all other apps. The `query` function in the `OMTG_CODING_003_SQL_Injection_Content_Provider_Implementation.java` class should be inspected.

`@Override public Cursor query(Uri uri, String[] projection, String selection,String[] selectionArgs, String sortOrder) {     SQLiteQueryBuilder qb = new SQLiteQueryBuilder();     qb.setTables(STUDENTS_TABLE_NAME);      switch (uriMatcher.match(uri)) {         case STUDENTS:             qb.setProjectionMap(STUDENTS_PROJECTION_MAP);             break;          case STUDENT_ID:             // SQL Injection when providing an ID             qb.appendWhere( _ID + "=" + uri.getPathSegments().get(1));             Log.e("appendWhere",uri.getPathSegments().get(1).toString());             break;          default:             throw new IllegalArgumentException("Unknown URI " + uri);     }      if (sortOrder == null || sortOrder == ""){         /**          * By default sort on student names          */         sortOrder = NAME;     }     Cursor c = qb.query(db, projection, selection, selectionArgs,null, null, sortOrder);      /**      * register to watch a content URI for changes      */     c.setNotificationUri(getContext().getContentResolver(), uri);     return c; }`

While the user is providing a STUDENT_ID at `content://sg.vp.owasp_mobile.provider.College/students`, the query statement is prone to SQL injection. Obviously [prepared statements ↗](https://cheatsheetseries.owasp.org/cheatsheets/SQL_Injection_Prevention_Cheat_Sheet.html "OWASP SQL Injection Prevention Cheat Sheet") must be used to avoid SQL injection, but [input validation ↗](https://cheatsheetseries.owasp.org/cheatsheets/Input_Validation_Cheat_Sheet.html "OWASP Input Validation Cheat Sheet") should also be applied so that only input that the app is expecting is processed.

All app functions that process data coming in through the UI should implement input validation:

- For user interface input, [Android Saripaar v2 ↗](https://github.com/ragunathjawahar/android-saripaar "Android Saripaar v2") can be used.
- For input from IPC or URL schemes, a validation function should be created. For example, the following determines whether the [string is alphanumeric ↗](https://stackoverflow.com/questions/11241690/regex-for-checking-if-a-string-is-strictly-alphanumeric "Input Validation"):

`public boolean isAlphaNumeric(String s){     String pattern= "^[a-zA-Z0-9]*$";     return s.matches(pattern); }`

An alternative to validation functions is type conversion, with, for example, `Integer.parseInt` if only integers are expected. The [OWASP Input Validation Cheat Sheet ↗](https://cheatsheetseries.owasp.org/cheatsheets/Input_Validation_Cheat_Sheet.html "OWASP Input Validation Cheat Sheet") contains more information about this topic.

## Dynamic Analysis

The tester should manually test the input fields with strings like `OR 1=1--` if, for example, a local SQL injection vulnerability has been identified.

On a rooted device, the command content can be used to query the data from a content provider. The following command queries the vulnerable function described above.

`# content query --uri content://sg.vp.owasp_mobile.provider.College/students`

SQL injection can be exploited with the following command. Instead of getting the record for Bob only, the user can retrieve all data.

`# content query --uri content://sg.vp.owasp_mobile.provider.College/students --where "name='Bob') OR 1=1--''"`

## Resources

- [Android Saripaar v2 ↗](https://github.com/ragunathjawahar/android-saripaar "Android Saripaar v2")
- [Input Validation ↗](https://stackoverflow.com/questions/11241690/regex-for-checking-if-a-string-is-strictly-alphanumeric "Input Validation")
- [OWASP Input Validation Cheat Sheet ↗](https://cheatsheetseries.owasp.org/cheatsheets/Input_Validation_Cheat_Sheet.html "OWASP Input Validation Cheat Sheet")
- [OWASP SQL Injection Prevention Cheat Sheet ↗](https://cheatsheetseries.owasp.org/cheatsheets/SQL_Injection_Prevention_Cheat_Sheet.html "OWASP SQL Injection Prevention Cheat Sheet")