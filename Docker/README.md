# 1. Install Docker 
```
curl -sSL https://get.docker.com | sh
```
# 2. Start a MySQL server instance (using hypriot/rpi-mysql img)
```
docker run --name name-of-container -e MYSQL_ROOT_PASSWORD=database-password -d hypriot/rpi-mysql:tag
```
  * name-of-container: replace with the name you want for your container.
  * database-password: replace with the database password for root user.
  * tag: specific version of MySQL or latest.

# 3. Connect to MySQL form an application in another Docker container (run container like this in order to connect to Mysql)
```
docker run --name name-of-app --link name-of-container:mysql -d app-that-uses-mysql
```
  * name-of-app:

# 4. Start portainer GUI
```
docker run --name portainer -d -p 9000:9000 -v /var/run/docker.sock:/var/run/docker.sock portainer/portainer
```
Then can go to http://yourDomain:9000 
