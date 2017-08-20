---
theme: post
title: "Enable PHP SQLite3 databse extension for Azure Web App"
categories: [azure]
tags: [php, azure, sqlite3]
---

# [](#intro)How to
* Log into your web app with FTP client of your choice
* Create new `ini` directory under `/site/` and enter it
* Make a new text file `settings.ini`
* Add to the file the following snippet (**see note below**):
```
extension=php_sqlite3.dll
sqlite3.extension_dir = "D:/Program Files (x86)/PHP/v7.1/ext"
```
* Save the file and close FTP
* Go to Portal Azure and open your web app's Application Settings
* In section App Settings add an entry with key `PHP_INI_SCAN_DIR` and value `d:\home\site\ini`
* Save and restart your web app from Overview screen

Your SQLite3 code should work now.

## [](#php_version)Beware PHP version
In the snippet there is a file path to the directory of a specific interpreter version. If you are using different version of PHP, you can try omitting this line. In case it doesn't work, you have to change the file path accordingly with the interpreter you use.

To check whether interpreter version you are using supports SQLite3, you can do the following:
* Go to https://<your-web-app>.scm.azurewebsites.net/DebugConsole
* Click on the __System Drive__ icon
* Follow to the `D:/Program Files (x86)/PHP/<your-php-version>/ext` directory
* Look for `php_sqlite3.dll` file

If you didn't find the .dll file, you can try finding it on the internet, uploading it to your app with FTP and pointing `settings.ini` to this directory.
