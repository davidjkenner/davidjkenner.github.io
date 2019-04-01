---
layout: post
title: Installing MySQL from the CLI
date: 2019-04-01 18:18 +0000
---
Install MySQL on Linux with the following commands, you will need to be root

{% highlight ruby %}
export DB_PWD=secret
export installDir=/opt
export dataDir=$installDir/mysql/data
export binDir=$installDir/mysql/bin 
declare BIT_APP='bitnami-mysql-8.0.15-0-linux-x64-installer.run'

cd /tmp
wget https://downloads.bitnami.com/files/stacks/mysql/8.0.15-0/$BIT_APP

chmod +x $BIT_APP               # makes file executable

$BIT_APP --mode text            # install from command line
{% endhighlight %}

Now review the installation
{% highlight ruby %}
# MySQL version with the command 
$installDir/mysql/bin/mysqladmin --version 
$installDir/mysql/bin/mysqld --version

cat $installDir/mysql/my.cnf           # The MySQL configuration file
 
cat $installDir/mysql/bitnami/my.cnf   # # Some configuration overrides  

# The MySQL .pid file allows other programs to find out the PID (Process Identification Number) of a running script. Find it at 
cat $installDir/mysql/data/mysqld.pid

# On Unix, MySQL clients can connect to the server in the local machine using an Unix socket file at 
cat $installDir/mysql/tmp/mysql.sock


# You can connect to the MySQL database from the same computer where it is installed with the mysql client tool.
cd $binDir
./mysql -u root -p$DB_PWD

# To back up all the databases, create a dump file using the mysqldump tool.
mysqldump -A -u root -p$DB_PWD > backup.sql

# Once you have the backup file, you can restore it with a command like the one below:
mysql -u root -p$DB_PWD < backup.sql

# The main log file is created at 
tail -n 100 $installDir/mysql/data/mysqld.log

# Start the MySQL database with the following command:
mysqld --skip-grant-tables --user=mysql --pid-file=/opt/bitnami/mysql/data/mysqld.pid 
 --skip-external-locking --port=3306 --sock=/opt/bitnami/mysql/tmp/mysql.sock

# Open a new console and try to log in the database:
mysql -u root -p 

# In this case, the error was related to the mysql.user table. Run these commands:
 mysql> use mysql;
 mysql> repair table user;
 mysql> check table user;
 mysql> exit;

###  How To Secure Your Server?
# Once you have created a new database and user for your application, connect to your MySQL server and follow these recommendations:
# Remove anonymous users:

 mysql> DELETE FROM mysql.user WHERE User='';
Remove the test database and access to it:

 mysql> DROP DATABASE test;
 mysql> DELETE FROM mysql.db WHERE Db='test' OR Db='test\\_%';
Disallow root login remotely:

 mysql> DELETE FROM mysql.user WHERE User='root' AND Host NOT IN ('localhost', '127.0.0.1', '::1');
Do not forget to reload the privileges tables to apply the changes:

 mysql> FLUSH PRIVILEGES;
Change your root user password.

It is strongly recommended that you do not have empty passwords for any user accounts when using the server for any production work.

If you do not need remote access, uncomment the line

 #bind-address=127.0.0.1
in the MySQL configuration file to only listen for connections on the local machine. Restart the server once done.
{% endhighlight %}