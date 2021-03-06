# Quick Start Guide to Install and Use ownCloud
This guide provides information about how to install ownCloud server using different methods, how to add user accounts, and how to connect to the ownCloud server using desktop or mobile clients.

# Introduction
ownCloud is an open source file synchronization and share software. This software caters to different types of users from free ownCloud Server edition users, to large enterprises and service providers operating the ownCloud Enterprise Subscription. ownCloud provides a safe, secure, and compliant file synchronization and sharing solution on servers that you control.
## Known Differences in ownCloud 10.0.10
ownCloud version 10.0.10 now has the following noteworthy changes:
*	Supports PHP 7.2
*	Provides new local user creation flow
*	Enables use of HTTP API for search functionality
*	Ensures enhanced security using native Brute-Force protection
*	Provides improved reliability for file upload feature
*	Provides option to prevent sharing with an undesirable group
*	Enables system administrators to reset or change password

For details about more changes, see Release Notes in [ownCloud Server Administration Guide](https://doc.owncloud.com/server/10.0/admin_manual/release_notes.html).
## Features of ownCloud 10.0.10
Some of the key features of ownCloud 10.0.10 are as follows:
*	Enables users to add the app to the Android homescreen
*	Compatible with PHP 7.1
*	Provides support for MySQL 4-byte UTF8: (utf8mb4 for example, Emoticons)  
*	Admin, personal pages, and app management are now merged together into a single "Settings" entry
*	Admin page displays the output of the server's status.php
*	Enables using email address for password recovery
*	Provides ability to disable password reset
*	Supports Redis Cluster
*	ownCloud log entry reorder
*	ownCloud log file rules to split into separate files
*	occ scanner optimized memory usage for large scans by using autocommits
*	Third party apps are not disabled anymore when upgrading

For more details, see [ownCloud 10.0 Features](https://github.com/owncloud/core/wiki/ownCloud-10.0-Features) page.
# System Requirements
Platform |	Options
----------|---------
Operating System |	Centos Linux 6 and 7,  Debian 7 and 8, Fedora 27 and 28, Red Hat Enterprise Linux 6 and 7, SUSE Linux Enterprise Server, 12 with SP1, SP2 and SP3, openSUSE Tumbleweed and Leap 15.0, 42.3, Ubuntu 16.04 and 18.04
Database	MySQL or MariaDB 5.5+ | Oracle 11g, PostgreSQL, SQLite
Web server | Apache 2.4 with prefork Multi-Processing Module (MPM) and mod_php
PHP Runtime*	| 5.6, 7.0, 7.1, and 7.2 (most preferred)
## Memory Requirements
Memory requirements for running an ownCloud server varies depending on the numbers of users and files, and volume of server activity. ownCloud requires a minimum of 128MB RAM, however, a minimum of 512MB is recommended.
## Database Requirements
Note the following requirements if you are running ownCloud together with a MySQL or MariaDB database:
*	Disabled or BINLOG_FORMAT = MIXED or BINLOG_FORMAT = ROW configured Binary Logging (See: [MySQL / MariaDB with Binary Logging Enabled](https://doc.owncloud.com/server/10.0/admin_manual/configuration/database/linux_database_configuration.html#db-binlog-label))
*	InnoDB storage engine (The MyISAM storage engine is not supported, see: [MySQL / MariaDB storage engine](https://doc.owncloud.com/server/10.0/admin_manual/configuration/database/linux_database_configuration.html#db-storage-engine-label))
* “READ COMMITTED” transaction isolation level (See: [MySQL / MariaDB “READ COMMITTED” transaction isolation level](https://doc.owncloud.com/server/10.0/admin_manual/configuration/database/linux_database_configuration.html#db-transaction-label))
See [deployment considerations](https://doc.owncloud.com/server/10.0/admin_manual/installation/deployment_considerations.html) and [deployment recommendations](https://doc.owncloud.com/server/10.0/admin_manual/installation/deployment_recommendations.html) for other considerations and recommendations.

# Installing ownCloud Server on Linux Using Package Manager
Install ownCloud on Linux from Open Build Service packages and you can use package manager to keep the ownCloud server up-to-date. If there are no packages for your Linux distribution, you can setup ownCloud from scratch using a classic LAMP stack (Linux, Apache, MySQL/MariaDB, PHP).
As the ownCloud tar archive contains all of the required third-party PHP libraries, there are not many prerequisites for installation. However, ownCloud require that PHP has a set of extensions installed, enabled, and configured. For more information, see [Prerequisites](https://doc.owncloud.com/server/10.0/admin_manual/installation/source_installation.html#prerequisites). 
## How to Install the Required Packages
You need to install the required modules before installing ownCloud using package manager.
### Modules on Ubuntu 16.04 LTS Server
On Ubuntu 16.04 LTS server, install the required modules for ownCloud installation by using Apache and MariaDB. Issue the following commands in a terminal:
   ``` 
   #If the add-apt-repository command is not available install software-properties-common
    sudo add-apt-repository ppa:ondrej/php
    sudo apt-get update
    sudo apt-get install -y apache2 mariadb-server libapache2-mod-php7.2 \
        openssl php-imagick php7.2-common php7.2-curl php7.2-gd \
        php7.2-imap php7.2-intl php7.2-json php7.2-ldap php7.2-mbstring \
        php7.2-mysql php7.2-pgsql php-smbclient php-ssh2 \
        php7.2-sqlite3 php7.2-xml php7.2-zip
```
**Note:**
*	php7.2-common provides: ftp, Phar, posix, iconv, ctype
*	The Hash extension is available from PHP 5.1.2 by default
*	php7.2-xml provides DOM, SimpleXML, XML, & XMLWriter
*	php7.2-zip provides zlib
### smbclient
To install smbclient, you can use the following script. It first installs PEAR, which at the time of writing installs only version 1.9.4. However, smbclient requires version 1.9.5. So, the final two commands upgrade PEAR to version 1.9.5 and then install smbclient using Pecl.
```
#!/usr/bin/expect
spawn wget -O /tmp/go-pear.phar http://pear.php.net/go-pear.phar
expect eof

spawn php /tmp/go-pear.phar

expect "1-11, 'all' or Enter to continue:"
send "\r"
expect eof

spawn rm /tmp/go-pear.phar

pear install PEAR-1.9.5
pecl install smbclient
```
### SSH2
To install SSH2, which provides SFTP, you can use the following command:
```
spawn pecl install ssh2
```

If you are planning to run additional apps, you might require additional packages. See [Prerequisites](https://doc.owncloud.com/server/10.0/admin_manual/installation/source_installation.html#prerequisites-label) for details.

**Note:**
During the installation of the MySQL/MariaDB server, you will be prompted to create a root password. Be sure to remember your password as you will need it during ownCloud database setup.
### Extensions on RedHat Enterprise Linux 7.2
On RedHat Enterprise Linux, you need the following extensions:
 ```
 # Enable the RHEL Server 7 repository
    subscription-manager repos --enable rhel-server-rhscl-7-eus-rpms

# Install the required packages
sudo yum install httpd mariadb-server php72 php72-php \
  php72-php-gd php72-php-mbstring php72-php-mysqlnd
```
## How to Install ownCloud Server
Perform the following steps to Install ownCloud server.
1.	Download the archive of the latest ownCloud version.
2.	Go to the ownCloud Download Page.
3.	Go to Download ownCloud Server > Download > Archive file for server owners and download either the tar.bz2 or .zip archive. 
This downloads a file named owncloud-x.y.z.tar.bz2 or owncloud-x.y.z.zip (where x.y.z is the version number).
4.	Download its corresponding checksum file. For example, owncloud-x.y.z.tar.bz2.md5, or owncloud-x.y.z.tar.bz2.sha256.
5.	Verify the MD5 or SHA256 sum.
```
md5sum -c owncloud-x.y.z.tar.bz2.md5 < owncloud-x.y.z.tar.bz2
sha256sum -c owncloud-x.y.z.tar.bz2.sha256 < owncloud-x.y.z.tar.bz2
md5sum  -c owncloud-x.y.z.zip.md5 < owncloud-x.y.z.zip
sha256sum  -c owncloud-x.y.z.zip.sha256 < owncloud-x.y.z.zip
```
6.	(Optional) Verify the PGP signature.
```
wget https://download.owncloud.org/community/owncloud-x.y.z.tar.bz2.asc
wget https://owncloud.org/owncloud.asc
gpg --import owncloud.asc
gpg --verify owncloud-x.y.z.tar.bz2.asc owncloud-x.y.z.tar.bz2
```
7.	Extract the archive contents. Run the appropriate unpacking command for your archive type.
    ```
    tar -xjf owncloud-x.y.z.tar.bz2
    ```
    ```
    unzip owncloud-x.y.z.zip
    ```
This unpacks to a single owncloud directory
8.	Copy the ownCloud directory to its final destination. When you are running the Apache HTTP server, you install ownCloud in your Apache document root.
```
cp -r owncloud /path/to/webserver/document-root
```
where /path/to/webserver/document-root is replaced by the document root of your Web server:
cp -r owncloud /var/www
On other HTTP servers, you must install ownCloud outside of the document root.
# How to Configure Apache Web Server
On Debian, Ubuntu, and their derivatives, Apache installs with a useful configuration, so all you have to do is create a ```/etc/apache2/sites-available/owncloud.conf``` file with these lines in it, replacing the Directory and other file paths with your own file paths:
```
Alias /owncloud "/var/www/owncloud/"```

<Directory /var/www/owncloud/>
  Options +FollowSymlinks
  AllowOverride All

 <IfModule mod_dav.c>
  Dav off
 </IfModule>

 SetEnv HOME /var/www/owncloud
 SetEnv HTTP_HOME /var/www/owncloud

</Directory>
```
Then create a symlink to ```/etc/apache2/sites-enabled:```

```
ln -s /etc/apache2/sites-available/owncloud.conf /etc/apache2/sites-enabled/owncloud.conf
```
In case you need additional configurations, see [Additional Apache Configurations](https://doc.owncloud.com/server/10.0/admin_manual/installation/source_installation.html#apache-configuration-label).
### Multi-Processing Module (MPM)
Apache prefork has to be used. Don’t use a threaded MPM like event or worker with mod_php, because PHP is currently not thread safe.
## How to Enable SSL
**Note:** You can use ownCloud over plain HTTP, but you must use SSL/TLS to encrypt all of your server traffic, and to protect user’s logins and data in transit.
Apache installed under Ubuntu comes with a simple self-signed certificate. All you have to do is to enable the ssl module and the default site. Open a terminal and run:
```a2enmod ssl
a2ensite default-ssl
service apache2 reload
```
**Note:** Self-signed certificates have their drawbacks when you plan to make your ownCloud server publicly accessible. Hence, you can get a certificate signed by a commercial signing authority.
## How to Run the Installation Wizard
After restarting Apache, you must complete your installation by running either the Graphical Installation Wizard or on the command line with the ```occ``` command. To enable this, temporarily change the ownership on your ownCloud directories to your HTTP user (see [Set Strong Directory Permissions](https://doc.owncloud.com/server/10.0/admin_manual/installation/source_installation.html#strong-perms-label) to learn how to find your HTTP user):
```
chown -R www-data:www-data /var/www/owncloud/
```
**Note:** Administrators of SELinux-enabled distributions might need to write new SELinux rules to complete the ownCloud installation.

**Warning:** ownCloud’s data directory must be exclusive to ownCloud and not be modified manually by any other process or user.
## How to Set Strong Directory Permissions
After completing the installation, you must set the directory permissions in your ownCloud installation. After you do so, your ownCloud server will be ready to use.
## How to Manage Trusted Domains
All URLs used to access your ownCloud server must be whitelisted in your ```config.php``` file, under the trusted_domains setting. Users are allowed to log into ownCloud only when they point their browsers to a URL that is listed in the trusted_domains setting.

**Note:** This setting is important when changing or moving to a new domain name. You may use IP addresses and domain names.
A typical configuration looks like this:
```
'trusted_domains' => [
   0 => 'localhost',
   1 => 'server1.example.com',
   2 => '192.168.1.50',
],
```
The loopback address, ```127.0.0.1```, is automatically whitelisted, so as long as you have access to the physical server you can always log in. In the event that a load-balancer is in place, there will be no issues as long as it sends the correct X-Forwarded-Host header.
For further information on improving the quality of your ownCloud installation, see [Configuration Notes and Tips](https://doc.owncloud.com/server/10.0/admin_manual/installation/configuration_notes_and_tips.html).
# Installing Using Command Line¶
ownCloud can be installed completely from the command line. This is convenient for scripted operations and for systems administrators who prefer using the command line over a GUI. It involves five steps:
1.	Ensure your server meets the [ownCloud prerequisites](https://doc.owncloud.com/server/10.0/admin_manual/installation/source_installation.html#prerequisites-label).
2.	Download and unpack the source.
3.	Install using the ```occ``` command.
4.	Set the correct owner and permissions.
5.	Optional post installation considerations.
To install ownCloud, first [download the source](https://owncloud.org/install/#instructions-server) (whether community or enterprise) directly from ownCloud, and then unpack (decompress) the tarball into the appropriate directory.
Next, you need to set your webserver user to be the owner of your unpacked owncloud directory, as in the example below.
```$ sudo chown -R www-data:www-data /var/www/owncloud/```
Use the ```occ``` command, from the root directory of the ownCloud source, to perform the installation. The following examples shows how to perform installation.
```
# Assuming you’ve unpacked the source to /var/www/owncloud/
$ cd /var/www/owncloud/
$ sudo -u www-data php occ maintenance:install \
   --database "mysql" --database-name "owncloud" \
   --database-user "root" --database-pass "password" \
   --admin-user "admin" --admin-pass "password"
```
**Note:** You must run occ as your HTTP user.
If you want to use a directory other than the default), you can supply the ```--data-dir``` switch. For example, if you are using the command above and you want the data directory to be ```/opt/owncloud/data```, then add ```--data-dir /opt/owncloud/data``` to the command.
When the command completes, apply the correct permissions to your ownCloud files and directories. This helps to protect your ownCloud installation and ensure that it operates correctly.
# Installing with Docker¶
ownCloud can also be installed using Docker, using the official [ownCloud Docker image](https://hub.docker.com/r/owncloud/server/). This image is designed to work with a data volume in the host filesystem and with separate *MariaDB* and *Redis containers*. The configuration exposes ```port 8080```, which allows for HTTP connections and mounts the data and MySQL data directories on the host for persistent storage.
# Installing on a Local Machine
To use the docker image, first you need to create a new project directory and download ```docker-compose.yml``` from the ownCloud Docker GitHub repository into the new directory. Next, create ```a.env``` configuration file, which contains the required configuration settings. The following settings are required:

Setting Name | Description |	Example
-------------|-------------|---------
OWNCLOUD_VERSION | The ownCloud version |	latest
OWNCLOUD_DOMAIN	| The ownCloud domain	| localhost
ADMIN_USERNAME	| The admin username	| admin
ADMIN_PASSWORD	| The admin user’s password	| admin
HTTP_PORT	| The HTTP port to bind to	| 8080

Then, you can start the container using your preferred Docker command-line tool. The following example shows how to use Docker Compose.
```
# Create a new project directory
mkdir owncloud-docker-server

cd owncloud-docker-server

# Copy docker-compose.yml from the GitHub repository
wget https://raw.githubusercontent.com/owncloud-docker/server/master/docker-compose.yml

# Create the environment configuration file
cat << EOF > .env
OWNCLOUD_VERSION=10.0
OWNCLOUD_DOMAIN=localhost
ADMIN_USERNAME=admin
ADMIN_PASSWORD=admin
HTTP_PORT=8080
EOF
# Build and start the container
docker-compose up -d
```
When the process completes, check whether all the containers have successfully started, by running the ```docker-compose ps``` command. The following example shows the output when all the containers have started:
```
 Name               Command                             State       Ports
-------------------------------------------------------------------------------------------------------
server_db_1         /usr/bin/entrypoint /bin/s ...      Up          3306/tcp
server_owncloud_1   /usr/local/bin/entrypoint  ...      Up          0.0.0.0:8080->8080/tcp
server_redis_1      /bin/s6-svscan /etc/s6              Up          6379/tcp
```
In the example, you can see that the database, ownCloud, and Redis containers are running, and that ownCloud is accessible through port 8080 on the host machine.
When all the containers are running, it takes a few minutes for ownCloud to be fully functional. If you run the ```docker-compose logs```, and see a significant amount of information logging to the console, then wait until it slows down to attempt to access the web UI.
# Logging In
To log in to the ownCloud UI, open ```https://localhost``` in your browser of choice. The ownCloud login screen is displayed.

![LoginPage](/docs/LogIn.png)

 Enter the username and password that you stored in ```.env``` earlier.
The first time that you access the login page through HTTPS, a browser warning appears, as the SSL certificate in the Docker setup is self-signed. However, the self-signed certificate can be overwritten with a valid certificate within the host volume.

# Adding a User Account
On the User management page of ownCloud Web UI, you can add a new user account.
User accounts have the following properties:

Property Name              |	      Description
---------------------------|---------------------------------------------------------
Login Name (Username)      |	The unique ID of an ownCloud user that cannot be changed.
Full Name |	The display name of the user that appears on file shares, the ownCloud Web interface, and emails. Administrators and users can change the Full Name any whenever needed. If the Full Name is not set, then a default name with the format, login name.ownCloud domain is assigned.
Password |	The administrator sets the first password for the user. Both the user and the administrator can change the password later.
Groups |	Group memberships are assigned to users. By default, new users are not assigned to any groups.
Group Admin |	Group admins are granted administrative privileges on specific groups, and can add and remove users from their groups.
Quota	| The maximum disk space assigned to each user. Any user that exceeds the quota cannot upload or sync data. You have the option to include external storage in user quotas.
## How to Add a New User
To create a user account, select **users** from the **users** drop down box and click the **Create** button as shown below.
![Create User](/docs/AddNewUser.png)
 
You need to enter new Login Name of the user and the initial Password. You can assign a group membership by selecting **Add Group** option.
Login names can contain: alphabets (a-z and A-Z), numbers (0-9), dashes (-), underscores (_), periods (.) and at signs (@). After creating the user, you can fill in their Full Name if the name is different from the login name, or leave it for the user to complete.
For more information, see [User Management in ownCloud Administration Guide](https://doc.owncloud.com/server/10.0/admin_manual/configuration/user/user_configuration.html).
 
# Connecting to the ownCloud Server Using the Desktop Client
As a User, you can connect and synchronize with the ownCloud server by using the ownCloud Desktop Synchronization client. 
The ownCloud desktop client enables you to:
*	Specify one or more directories on your computer that you want to synchronize to the ownCloud server.
*	Always have the latest files synchronized, wherever they are located.
Your files are always automatically synchronized between your ownCloud server and local PC. There are clients for Linux, macOS, and Microsoft Windows.
You can use MSI installer to install ownCloud desktop client. This client provides several features that can be installed or removed individually. which you can also control through command-line. If you are automating the installation, then run the following command:

```msiexec /passive /i ownCloud-x.y.z.msi```

If you just want to install ownCloud Desktop Synchronization Client on your local system, you can simply launch the `.msi` file and configure it in the wizard that pops up. For more information, see [ownCloud Client manual](https://doc.owncloud.org/desktop/latest/).
# Connecting to the ownCloud Server Using the Mobile Client
Using web interface to access files on ownCloud is easy and convenient as you can use any Web browser on any operating system without installing special client software. However, the ownCloud Android app offers some advantages over the Web interface:
*	A simplified interface that fits nicely on a tablet or smartphone
*	Automatic synchronization of your files
*	Share files with other ownCloud users and groups, and create multiple public share links
*	Upload of photos and videos recorded on your Android device
*	Easily add files from your device to ownCloud
*	Two-factor authentication

You can get ownCloud Android app by logging into your ownCloud server from your Android device using a Web browser such as Chrome, Firefox, or Dolphin. For more information see [ownCloud Android App Manual](https://doc.owncloud.org/android/).
