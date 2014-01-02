---
layout: post
title: mysqldump问题
category: blog
description: mysqldump的版本和当前数据库的版本不一致就会导致备份/恢复问题
---

问题如下：

	WARNING
	mysqldump.exe is version 5.5.16, but the MySQL Server to be dumped has version 5.6.11.
	Because the version of mysqldump is older than the server, some features may not be backed up properly.
	It is recommended you upgrade your local MySQL client programs, including mysqldump to a version equal to or newer than that of the target server.
	The path to the dump tool must then be set in Preferences -> Administrator -> Path to mysqldump Tool:
	12:35:48 Dumping shapefiles (countries)
	Running: mysqldump.exe --defaults-extra-file="c:\users\shixz\appdata\local\temp\5\tmppn0cgl.cnf"  --user=root --max_allowed_packet=1G --host=localhost --port=3307 --default-character-set=utf8 "shapefiles" "countries"
	mysqldump: Couldn't execute 'SET OPTION SQL_QUOTE_SHOW_CREATE=1': You have an error in your SQL syntax; check the manual that corresponds to your MySQL server version for the right syntax to use near 'OPTION SQL_QUOTE_SHOW_CREATE=1' at line 1 (1064)
	Operation failed with exitcode 2
	12:35:48 Dumping shapefiles (cities)
	Running: mysqldump.exe --defaults-extra-file="c:\users\shixz\appdata\local\temp\5\tmpgjfy9u.cnf"  --user=root --max_allowed_packet=1G --host=localhost --port=3307 --default-character-set=utf8 "shapefiles" "cities"
	mysqldump: Couldn't execute 'SET OPTION SQL_QUOTE_SHOW_CREATE=1': You have an error in your SQL syntax; check the manual that corresponds to your MySQL server version for the right syntax to use near 'OPTION SQL_QUOTE_SHOW_CREATE=1' at line 1 (1064)
	Operation failed with exitcode 2
	12:35:48 Dumping shapefiles (rivers)
	Running: mysqldump.exe --defaults-extra-file="c:\users\shixz\appdata\local\temp\5\tmpiehbkp.cnf"  --user=root --max_allowed_packet=1G --host=localhost --port=3307 --default-character-set=utf8 "shapefiles" "rivers"
	mysqldump: Couldn't execute 'SET OPTION SQL_QUOTE_SHOW_CREATE=1': You have an error in your SQL syntax; check the manual that corresponds to your MySQL server version for the right syntax to use near 'OPTION SQL_QUOTE_SHOW_CREATE=1' at line 1 (1064)
	Operation failed with exitcode 2
	12:35:48 Export of C:\Users\shixz\Documents\dumps\Dump20130530 has finished with 3 errors

提示就是说mysqldump的版本和当前数据库的版本不一致，解决方法就是把MySql安装目录中的bin下的mysqldump.exe复制放到MySQL Workbench CE的安装目录下，并且覆盖掉原来的mysqldump.exe
