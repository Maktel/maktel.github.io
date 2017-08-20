---
theme: post
title: "Enable PHP SQLite3 database extension for Azure Web App"
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
* Go to `https://<your-web-app>.scm.azurewebsites.net/DebugConsole`
* Click on the _System Drive_ icon
* Follow to the `D:/Program Files (x86)/PHP/<your-php-version>/ext` directory
* Look for `php_sqlite3.dll` file

If you didn't find the .dll file, you can try finding it on the internet, uploading it to your app with FTP (for instance to `/site/bin/`) and pointing `settings.ini` to this directory.

# [](#why)Why would you want to do this?
I was looking for a quick way to store information in a database for a small PHP project. Full blown database seemed to be an overkill, since my project used file storage with much success. I have learnt about the **\***built-in database and happily wrote my code with local PHP development server. After deployment it turned out Azure didn't support it out of the box, so instead of using different, universal database solution, I spent two hours trying to configure environment. It was worth it

**\*** by built-in I mean `sudo apt install php-sqlite3`
