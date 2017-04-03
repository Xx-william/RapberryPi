# Preperation
```
sudo apt-get update
sudo apt-get upgrade
```
## 1.1 install vim
```
sudo apt-get install vim
```
## 1.2Change RP's host name and domain name
```
sudo vim /etc/hosts
```
Change raspberry to your host name (like Designbeasts)
```
sudo vim /etc/hostname
```
Change raspberry to your host name (like Designbeasts)
```
sudo /etc/init.d/hostname.sh
sudo reboot
```
## 1.3 Install TightVNCServer
```
sudo apt-get install tightvncserver
```
Then you can share your RP's screen with Mac use
```
vnc://hostname:5901
```
## 1.4 Install FTP server( optional )
```
sudo apt-get install pure-ftpd
sudo groupadd ftpgroup
sudo useradd ftpuser -g ftpgroup -s /sbin/nologin -d /dev/null
sudo mkdir /home/pi/FTP
sudo chown -R ftpuser:ftpgroup /home/pi/FTP
sudo pure-pw useradd wangxi -u ftpuser -g ftpgroup -d /home/pi/FTP -m
sudo pure-pw mkdb
sudo ln -s /etc/pure-ftpd/conf/PureDB /etc/pure-ftpd/auth/60puredb
sudo service pure-ftpd restart
```

See more details [Here](https://download.pureftpd.org/pub/pure-ftpd/doc/README.Virtual-Users)

## 1.5 Enable SSH
```
sudo raspi-config
```
Navigate to the **Interface** tab, select **Enable** next to **SSH**
Then you can connect to you pi with FTP client using protocol SFTP

# 2. LAMP
## 2.1 Install Apache
```
sudo apt-get install apache2 -y
sudo service apache2 restart
```
## 2.2 Install PHP
```
sudo apt-get install php5 libapache2-mod-php5 -y
sudo service apache2 restart
```
## 2.3 Install MySQL
```
sudo apt-get install mysql-server php5-mysql -y
sudo service apache2 restart
```
Need to set your password while instalation

## 2.4 MySQL configuration
```
mysql -uroot -p
```
```
create database wordpress;
```

# 3. Install Wordpress
```
cd /var/www/html
sudo chown pi: .
sudo chown -R pi:www-data /var/www/html
```
At this part, you can either upload [Wordpress](https://github.com/WordPress/WordPress) to **/var/www/html** or download via terminal
```
wget https://wordpress.org/latest.tar.gz -P /var/www/html
tar xzf latest.tar.gz
mv wordpress/* .
rm -rf latest.tar.gz wordpress
```
After we download/upload the wordpress file, now we need to do some modification to **wp-config-sample.php** 
```
vim wp-config-sample.php
```
```/** The name of the database for WordPress */
define('DB_NAME', 'database_name_here');

/** MySQL database username */
define('DB_USER', 'username_here');

/** MySQL database password */
define('DB_PASSWORD', 'password_here');
```
Replace **'database_name_here'**, **'username_here'**, **'password_here'** with your database info.
```
mv wp-config-sample.php wp-config.php
```

One more thing:
If you want to dev your wordpress site at local, you need to add the code below into **wp-config.php**, then you can install the wordpress plugin at local env
```
define('FS_METHOD', 'direct');
```
Now you are good to go.
