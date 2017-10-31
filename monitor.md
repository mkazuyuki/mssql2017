# EXPRESSCLUSTER QSG for SQL Server 2017 for Linux

1. Product versions
	- CentOS 7.4 (kernel-3.10.0-693.el7.x86_64)
	- mssql-server-14.0.3006.16-3.x86_64
	- mssql-tools-14.0.6.0-1.x86_64

2. Procedure

	1. Install OS
	2. [Install MSSQL and tools then setup.](https://docs.microsoft.com/ja-jp/sql/linux/quickstart-install-connect-red-hat)
	3. Install ECX (SSS) then setup.

3. Create a table in the Database to be monitored

	- *TARGET_DB*	= the database name to be monitored
	- *PASSWORD*	= the password for SA user to connect the database

			sqlcmd -U SA -P PASSWORD -Q "USE *TARGET_DB*; CREATE TABLE CLPMON (id INT); INSERT INTO CLPMON VALUES (1)"

4. Add *Custom Monitor Resource* to the cluster and edit genw.sh as

		sqlcmd -U SA -P PASSWORD -Q "USE *TARGET_DB*; SELSCT * FROM CLPMON WHERE id = 1"|grep id
		$if [ $? -ne 0 ]; then
			echo "[E] sqlcmd returns unexpected result"
			exit 1:
		fi
