# 1. Upgrate Python to latest version ( Optional )
Install library needed
```
 sudo apt-get install build-essential \
                       libncursesw5-dev \
                       libreadline6-dev \
                       libssl-dev \
                       libgdbm-dev \
                       libc6-dev \
                       libsqlite3-dev tk-dev \
                       libbz2-dev
```
Then download latest version of Python
```
cd ~
wget https://www.python.org/ftp/python/3.6.1/Python-3.6.1.tgz 
```
Here you can change the version of the Python you want to install (upgrade) by replace the version number '/3.5.1/Python-3.5.1.tgz' in that url
```
tar -zxvf Python-3.6.1.tgz
cd Python-3.6.1
./configure
make
sudo make install
```
The step 'make' takes me 3 mins to finish
The step 'make' takes me about 13 mins to finish
The step 'make install' takes me 3 min to finish
It may depends on your raspberry pi's version and the microSD card you are using. (I'm using pi 3 B+ and Sandisk 64G SDXC)

After all done, don't forget to reboot your raspberry pi
```
sudo reboot
```

# 2. Install packages
If the version of Python you are using is under 3.0, go check
[here](https://www.digitalocean.com/community/tutorials/how-to-serve-django-applications-with-apache-and-mod_wsgi-on-ubuntu-14-04).
If the version of Python you are using is beyond 3.0, continu.
```
sudo apt-get update
sudo apt-get install python3-pip apache2 libapache2-mod-wsgi-py3
```
*Reminder: When operating outside of a virtual env, use command python3 and pip3 instead of python and pip.

# 3. Install and configure Virtual enviroment
  ```
  sudo pip3 install virtualenv
  ```
  After your virtual env is installed, create a directory where you want to keep your project.
  
  ```
  mkdir ~/pyproject
  cd ~/pyproject
  ```
  In the project dir, create a py virtual env
  ```
  virtualenv myprojectenv
  ```
  Activate the virtual env
  ```
  source myprojectenv/bin/activate
  ```
  
# 4. Install Django in virtual env
  When your virtual env is activate, you dont need to use the command pip3 or python3, just use pip or python,
  the virtual env will using the correct version automaticly
  ```
  pip install django
  ```

# 5. Create and configure a new Django project
Be noticed that you are always under virtual env.
```
django-admin.py startproject mydjgproject .
```
It will create a Django project under the current directory.

  ## 5.1 Adjust the project setting
  Setting path of static file
  ```
  vim mydjgproject/settings.py
  ```
  Add the following line into the bottom of settings.py file
  ```
  STATIC_ROOT = os.path.join(BASE_DIR, "static/")
  ```
  ## 5.2 Complete initial project setup
  ### Create SQLite database
  ```
  cd ~/myproject
  ./manage.py makemigrations
  ./manage.py migrate
  ```
  ### Create an admin acount for adim module in Django
  ```
  ./manage.py createsuperuser
  ```
  Enter the information asked.
  
  ### Collect all of the static content into the directory location we configured
  ```
  ./manage.py collectstatic
  ```
  ## 5.3 Test the project
  If you've changed pi's host name, you need to add host to into
  ```
  ALLOWED_HOSTS = []
  ```
  in the settings.py
  ```
  ./manage.py runserver 0.0.0.0:8000
  ```
  The go to the browser, enter your urls with port 8000 see if it works.
  
  If it works, ctrl + c to stop the server and leave the virtual env
  ```
  deactivate
  ```
  # 6. Congigure Apache
  Before doing this, it's better to make a copy of file 000-default.conf
  ```
  sudo vim /etc/apache2/sites-available/000-default.conf
  ```
  
  Add the following line to the bottom 
  ```
  <VirtualHost *:80>
  
	 ...
	 
	Alias /static /home/pi/pyproject/static
          <Directory /home/pi/pyproject/static>
              Require all granted
          </Directory>

          <Directory /home/pi/pyproject/mydjgproject>
              <Files wsgi.py>
                  Require all granted
              </Files>
          </Directory>

    WSGIDaemonProcess mydjgproject python-path=/home/pi/pyproject python-home=/home/pi/pyproject/myprojectenv
    WSGIProcessGroup mydjgproject
    WSGIScriptAlias / /home/pi/pyproject/mydjgproject/wsgi.py
	
</VirtualHost>
  ```
  
  ### Wrapping up some permissions issues
  ```
  chmod 664 ~/pyproject/db.sqlite3
  sudo chown :www-data ~/pyproject/db.sqlite3
  sudo chown :www-data ~/pyproject
  ```
  restart apache2 server, before doing this. please reload the daemon
  ```
  sudo systemctl daemon-reload
  sudo service apache2 restart
  ```
  
  Now you are good to go.