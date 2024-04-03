## Overview

## Static Analysis

### Local

Check the `AndroidManifest.xml` file for the following flag:

`android:allowBackup="true"`

If the flag value is **true**, determine whether the app saves any kind of sensitive data (check the test case "Testing for Sensitive Data in Local Storage").

### Cloud

Regardless of whether you use key/value backup or auto backup, you must determine the following:

- which files are sent to the cloud (e.g., SharedPreferences)
- whether the files contain sensitive information
- whether sensitive information is encrypted before being sent to the cloud.

> If you don't want to share files with Google Cloud, you can exclude them from [Auto Backup ↗](https://developer.android.com/guide/topics/data/autobackup.html#IncludingFiles "Exclude files from Auto Backup"). Sensitive information stored at rest on the device should be encrypted before being sent to the cloud.

- **Auto Backup**: You configure Auto Backup via the boolean attribute `android:allowBackup` within the application's manifest file. [Auto Backup ↗](https://developer.android.com/guide/topics/data/autobackup.html#EnablingAutoBackup "Enabling AutoBackup") is enabled by default for applications that target Android 6.0 (API level 23). You can use the attribute `android:fullBackupOnly` to activate auto backup when implementing a backup agent, but this attribute is available for Android versions 6.0 and above only. Other Android versions use key/value backup instead.

`android:fullBackupOnly`

Auto backup includes almost all the app files and stores up 25 MB of them per app in the user's Google Drive account. Only the most recent backup is stored; the previous backup is deleted.

- **Key/Value Backup**: To enable key/value backup, you must define the backup agent in the manifest file. Look in `AndroidManifest.xml` for the following attribute:

`android:backupAgent`

To implement key/value backup, extend one of the following classes:

- [BackupAgent ↗](https://developer.android.com/reference/android/app/backup/BackupAgent.html "BackupAgent")
- [BackupAgentHelper ↗](https://developer.android.com/reference/android/app/backup/BackupAgentHelper.html "BackupAgentHelper")

To check for key/value backup implementations, look for these classes in the source code.

## Dynamic Analysis

After executing all available app functions, attempt to back up via `adb`. If the backup is successful, inspect the backup archive for sensitive data. Open a terminal and run the following command:

`adb backup -apk -nosystem <package-name>`

ADB should respond now with "Now unlock your device and confirm the backup operation" and you should be asked on the Android phone for a password. This is an optional step and you don't need to provide one. If the phone does not prompt this message, try the following command including the quotes:

`adb backup "-apk -nosystem <package-name>"`

The problem happens when your device has an adb version prior to 1.0.31. If that's the case you must use an adb version of 1.0.31 also on your host computer. Versions of adb after 1.0.32 [broke the backwards compatibility. ↗](https://issuetracker.google.com/issues/37096097 "adb backup is broken since ADB version 1.0.32")

Approve the backup from your device by selecting the _Back up my data_ option. After the backup process is finished, the file _.ab_ will be in your working directory. Run the following command to convert the .ab file to tar.

`dd if=mybackup.ab bs=24 skip=1|openssl zlib -d > mybackup.tar`

In case you get the error `openssl:Error: 'zlib' is an invalid command.` you can try to use Python instead.

`dd if=backup.ab bs=1 skip=24 | python -c "import zlib,sys;sys.stdout.write(zlib.decompress(sys.stdin.read()))" > backup.tar`

The [_Android Backup Extractor_ ↗](https://github.com/nelenkov/android-backup-extractor "Android Backup Extractor") is another alternative backup tool. To make the tool to work, you have to download the Oracle JCE Unlimited Strength Jurisdiction Policy Files for [JRE7 ↗](https://www.oracle.com/technetwork/java/javase/downloads/jce-7-download-432124.html "Oracle JCE Unlimited Strength Jurisdiction Policy Files JRE7") or [JRE8 ↗](http://www.oracle.com/technetwork/java/javase/downloads/jce8-download-2133166.html "Oracle JCE Unlimited Strength Jurisdiction Policy Files JRE8") and place them in the JRE lib/security folder. Run the following command to convert the tar file:

`java -jar abe.jar unpack backup.ab`

if it shows some Cipher information and usage, which means it hasn't unpacked successfully. In this case you can give a try with more arguments:

`abe [-debug] [-useenv=yourenv] unpack <backup.ab> <backup.tar> [password]`

`[password]` is the password when your android device asked you earlier. For example here is: 123

`java -jar abe.jar unpack backup.ab backup.tar 123`

Extract the tar file to your working directory.

`tar xvf mybackup.tar`

## Resources

- [Android Backup Extractor ↗](https://github.com/nelenkov/android-backup-extractor "Android Backup Extractor")
- [BackupAgentHelper ↗](https://developer.android.com/reference/android/app/backup/BackupAgentHelper.html "BackupAgentHelper")
- [BackupAgent ↗](https://developer.android.com/reference/android/app/backup/BackupAgent.html "BackupAgent")
- [Enabling AutoBackup ↗](https://developer.android.com/guide/topics/data/autobackup.html#EnablingAutoBackup "Enabling AutoBackup")
- [Exclude files from Auto Backup ↗](https://developer.android.com/guide/topics/data/autobackup.html#IncludingFiles "Exclude files from Auto Backup")
- [Oracle JCE Unlimited Strength Jurisdiction Policy Files JRE7 ↗](https://www.oracle.com/technetwork/java/javase/downloads/jce-7-download-432124.html "Oracle JCE Unlimited Strength Jurisdiction Policy Files JRE7")
- [Oracle JCE Unlimited Strength Jurisdiction Policy Files JRE8 ↗](http://www.oracle.com/technetwork/java/javase/downloads/jce8-download-2133166.html "Oracle JCE Unlimited Strength Jurisdiction Policy Files JRE8")
- [adb backup is broken since ADB version 1.0.32 ↗](https://issuetracker.google.com/issues/37096097 "adb backup is broken since ADB version 1.0.32")