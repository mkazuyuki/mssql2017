# EXPRESSCLUSTER Quick Start Guide - SQL Server 2017 for Linux

1. Versions verified

	- mssql-server-14.0.3006.16-3.x86_64
	- mssql-tools-14.0.6.0-1.x86_64
	- EXPRESSCLUSTER X (SSS) 3.3.5-1
	- CentOS 7.4 (kernel-3.10.0-693.el7.x86_64)

2. Overall Procedure

	1. Prepare HW
	2. Install and setup OS
	3. Install MSSQL and tools then setup.[*1]
	4. Install ECX (SSS) then setup.

3. Procedure for seting up ECX monitor resource

	1. On the OS console, create a table in the Database to be monitored by the script below

			#!/bin/sh
			# TARGET_DB	= the database name to be monitored  
			# PASSWORD	= the password for SA user to connect the database

			sqlcmd -U SA -P PASSWORD -Q "USE TARGET_DB; CREATE TABLE CLPMON (id INT); INSERT INTO CLPMON VALUES (1)"

	2. On the Cluster Mangaer, add *Custom Monitor Resource* to the cluster and edit genw.sh as below

			#!/bin/sh
			# TARGET_DB	= the database name to be monitored  
			# PASSWORD	= the password for SA user to connect the database

			sqlcmd -U SA -P PASSWORD -Q "USE TARGET_DB; SELSCT * FROM CLPMON WHERE id = 1"|grep id
			$if [ $? -ne 0 ]; then
				echo "[E] sqlcmd returned unexpected result"
				exit 1:
			fi


[*1]: https://docs.microsoft.com/ja-jp/sql/linux/quickstart-install-connect-red-hat
