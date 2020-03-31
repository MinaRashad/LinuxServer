# LinuxServer
## Secure linux server for hosting item catalog site
### The is server is currently down

ip: ~~52.58.37.72~~

ssh port : 2222

to browse the website propaply [because google sign-in does not accept ips as origins]

you have to edit your hosts file (found in /etc/hosts) and add the following line

`*Server IP*     restaurants-Item-Catalog`

url:http://restaurants-item-catalog/

put the RSA private key found in the notes sent with submition on your computer in the following directory : 

~/.ssh/catalog [where catalog is a file]
```
nano ~/.ssh/catalog
```
paste the private key here

Software installed:

1.apache2

2.sqlite3 (instead of postgreSQL) 

3.python3-pip

4.tree (to see the all directories)

5.mod_wsgi

## steps made for each rubric

### Can you log into the server as the user grader using the submitted key?
First Created grader user
```
sudo adduser grader
sudo su - grader
mkdir .ssh
touch .ssh/authorized_keys 
nano .ssh/authorized_keys
```
then added the Public key generated on my machine
## Is remote login of the root user disabled?
## Are users required to authenticate using RSA keys?
## is SSH hosted on non-default port?
These are all done by editing `/etc/ssh/sshd_config`

1.change PermitRootLogin to no.

2.change PasswordAuthentication it to no.

3.change the Port to 2222

### Is the grader user given sudo access?
this is done by adding grader to `/etc/sudoers.d/`
`sudo nano /etc/sudoers.d/grader`
and type 
`grader ALL=(ALL) NOPASSWD:ALL`

### Is the firewall configured to only allow for SSH, HTTP, and NTP?
this is done by typing the following set of firewall rules
```
sudo ufw default deny incoming
sudo ufw default allow outgoing
sudo ufw allow 2222/tcp
sudo ufw allow www
sudo ufw allow ntp
sudo ufw enable
```
### Are the applications up-to-date?
Run the Following command
`sudo apt update && sudo apt upgrade`
to upgrade to latest version

## Is there a web server running on port 80?
## Has the web server been configured to serve the Item Catalog application?
## Has the database server been configured to properly serve data?
*IMPORTANT NOTE: I Used SQLite3 package instead of postgreSQL package as it is the library I used in my Web app*

this is done by creating conf file with
`sudo nano /etc/apache2/sites-available/FlaskApp.conf` 
and type the following
```
<VirtualHost *:80>
		ServerName restaurants-item-catalog/
		ServerAdmin admin@test.com
		WSGIScriptAlias / /var/www/FlaskApp/flaskapp.wsgi
		<Directory /var/www/FlaskApp/FlaskApp/>
			WSGIProcessGroup FlaskApp
                        WSGIApplicationGroup %{GLOBAL}
			Order allow,deny
			Allow from all
		</Directory>
		Alias /static /var/www/FlaskApp/FlaskApp/static
		<Directory /var/www/FlaskApp/FlaskApp/static/>
			Order allow,deny
			Allow from all
		</Directory>
		
		ErrorLog /var/www/FlaskApp/logs/error.log
		LogLevel warn
		CustomLog /var/www/FlaskApp/logs/access.log combined
</VirtualHost>

```
then 
```
cd /var/www
sudo mkdir FlaskApp
cd FlaskApp
sudo mkdir FlaskApp
git clone https://github.com/MinaRashad/items-catalog.git ./
cd ../..
chown -R grader:grader FlaskApp
chmod -R a+rw FlaskApp
cd FlaskApp
mkdir logs
nano flaskapp.wsgi
```
then typed the following lines

```
#!/usr/bin/python3
import sys
#sys.stdout = sys.stderr
sys.path.append('/usr/local/lib/python3.6/dist-packages/')
sys.path.insert(0,"/var/www/FlaskApp")
from FlaskApp import app as application
application.secret_key = 'S3cr3T'
```
Finally I setup and tested the app to see if it is running by:
```
cd /var/www/FlaskApp/FlaskApp
mv main.py __init__.py
sudo pip3 install -r requirements
python3 setup_db.py
nano __init__.py
```
then changed the following 

1.the port from `2005` to `80` to not change ufw rules

2.the directory for the db instead of relative path `./restaurant.db` to absolute path `/var/www/FlaskApp/FlaskApp/restaurant.db` 

tested it by running `sudo python3 __init__.py` and it ran successfuly with the ability to run and serve the app
It also can Serve database data and read and write from it.

#### References
1. https://www.digitalocean.com/community/tutorials/how-to-deploy-a-flask-application-on-an-ubuntu-vps

2. https://askubuntu.com
