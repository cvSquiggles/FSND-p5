### Udacity Full-stack Web Dev. - Project 5

## Project Status

I've attempted this from scratch 3 times now following different guides, and everytime, when i get to the end,
I just get a 500 internal server error. I've looked through the error log files, and I'm having trouble finding
the source of the problem, here's the error.log report:

```
[Wed Jul 31 04:59:52.609298 2019] [wsgi:error] [pid 2888:tid 139975696873216]     compat.reraise(exc_type, exc_value, exc_tb)
[Wed Jul 31 04:59:52.609300 2019] [wsgi:error] [pid 2888:tid 139975696873216]   File "/usr/local/lib/python2.7/dist-packages/sqlalchemy/pool/impl.py", line 136, in _do_get
[Wed Jul 31 04:59:52.609302 2019] [wsgi:error] [pid 2888:tid 139975696873216]     return self._create_connection()
[Wed Jul 31 04:59:52.609304 2019] [wsgi:error] [pid 2888:tid 139975696873216]   File "/usr/local/lib/python2.7/dist-packages/sqlalchemy/pool/base.py", line 308, in _create_connection
[Wed Jul 31 04:59:52.609307 2019] [wsgi:error] [pid 2888:tid 139975696873216]     return _ConnectionRecord(self)
[Wed Jul 31 04:59:52.609309 2019] [wsgi:error] [pid 2888:tid 139975696873216]   File "/usr/local/lib/python2.7/dist-packages/sqlalchemy/pool/base.py", line 437, in __init__
[Wed Jul 31 04:59:52.609311 2019] [wsgi:error] [pid 2888:tid 139975696873216]     self.__connect(first_connect_check=True)
[Wed Jul 31 04:59:52.609313 2019] [wsgi:error] [pid 2888:tid 139975696873216]   File "/usr/local/lib/python2.7/dist-packages/sqlalchemy/pool/base.py", line 639, in __connect
[Wed Jul 31 04:59:52.609315 2019] [wsgi:error] [pid 2888:tid 139975696873216]     connection = pool._invoke_creator(self)
[Wed Jul 31 04:59:52.609318 2019] [wsgi:error] [pid 2888:tid 139975696873216]   File "/usr/local/lib/python2.7/dist-packages/sqlalchemy/engine/strategies.py", line 114, in connect
[Wed Jul 31 04:59:52.609322 2019] [wsgi:error] [pid 2888:tid 139975696873216]     return dialect.connect(*cargs, **cparams)
[Wed Jul 31 04:59:52.609325 2019] [wsgi:error] [pid 2888:tid 139975696873216]   File "/usr/local/lib/python2.7/dist-packages/sqlalchemy/engine/default.py", line 453, in connect
[Wed Jul 31 04:59:52.609327 2019] [wsgi:error] [pid 2888:tid 139975696873216]     return self.dbapi.connect(*cargs, **cparams)
[Wed Jul 31 04:59:52.609329 2019] [wsgi:error] [pid 2888:tid 139975696873216]   File "/usr/local/lib/python2.7/dist-packages/psycopg2/__init__.py", line 125, in connect
[Wed Jul 31 04:59:52.609331 2019] [wsgi:error] [pid 2888:tid 139975696873216]     dsn = _ext.make_dsn(dsn, **kwargs)
[Wed Jul 31 04:59:52.609333 2019] [wsgi:error] [pid 2888:tid 139975696873216]   File "/usr/local/lib/python2.7/dist-packages/psycopg2/extensions.py", line 174, in make_dsn
[Wed Jul 31 04:59:52.609336 2019] [wsgi:error] [pid 2888:tid 139975696873216]     parse_dsn(dsn)
[Wed Jul 31 04:59:52.609338 2019] [wsgi:error] [pid 2888:tid 139975696873216] ProgrammingError: (psycopg2.ProgrammingError) invalid dsn: invalid connection option "check_same_thread"
[Wed Jul 31 04:59:52.609340 2019] [wsgi:error] [pid 2888:tid 139975696873216]
[Wed Jul 31 04:59:52.609342 2019] [wsgi:error] [pid 2888:tid 139975696873216] (Background on this error at: http://sqlalche.me/e/f405)
```

My project runs fine through flask on localhost, and I see the correct tables in my postgres db,  so that's not the issue.

I'm really worried, as there are only a few weeks left in the class, and I really could use some guidance!

## Grader Info

Host Name: ec2-35-182-93-218.ca-central-1.compute.amazonaws.com

IP Address: 35.182.93.218.xip.io

keyPassword: 123456

## Steps

# Amazon Lightsail Instance Set Up

  1. Navigate and sign in to Amazon Lightsail console.
  
  2. Click *Create Instance* to create a new server instance.
  
  3. Select *Linux/Unix* and *OS Only*, then choose *Ubuntu 16.04 LTS*
  
  4. Name the instance something unique.
  
  5. After the server status is *Running* click it to manage it.
  
  6. Click the *Account page* link near the bottom of the page to download the .pem key file to sign in with later.
  
  7. Then add Ports 123, and 2200 to the firewall via the *Networking* management tab, and save the changes.
  
# Server Configuration

  1. Place the *.pem* public key file into the *~/users/myUserName/.ssh/* folder.
  
  2. Use **chmod 600** to secure the file.
  
  3. You can now use this key to login to your server through BASH: **ssh -i ~/.ssh/*.pem* ubuntu@35.182.93.218**
  
  4. Use **sudo su -** to switch to root user briefly.
  
  5. Immediately create the *grader* user that Udacity requested with: **sudo adduser grader** (password set to 123456)
  
  6. After that create a file in the sodoers dir with: **sudo nano /etc/sudoers.d/grader
    6a. Type ```grader ALL=(ALL:ALL) ALL``` into the file and save it with **ctrl+x**, **y**, **enter**.
  
  7. Per chuanqin3's guide a workaround for a potential host resolution error,  **sudo nano /etc/hosts**
    7a. Then add ```127.0.1.1 ip-10-20-37-65``` under ```127.0.1.1:localhost```.
  
  8. Update packages
    8a. ```sudo apt-get update```
    8b. ```sudo apt-get upgrade```
    8c. ```sudo apt-get install finger``` (Makes managing users simpler)
    
  9. In **ANOTHER** bash, run ```ssh-keygen -f ~/.ssh/udacity_key.rsa```
    9a. Use ```cat ~/.ssh/udacity_key.rsa.pub``` to read the newly created key and copy it to your clipboard.
    
  10. **Now back in your server connected bash** cd  to */home/grader*
  
  11. Create a *.ssh* directory there with ```mkdir .ssh```
    11a. Create a file to save the server side public key: ```touch .ssh/authorized_keys```
    11b. Edit that file and paste the previously copied key in one line and save it.
  
  12. Change permissions on the .ssh folder and authorized_keys file to 700, and 644 respectively.
    12a. Also chown from root to grader with: ```sudo chown -R grader:grader /home/grader/.ssh```
    
  13. At this point restart your ssh with: ```sudo service ssh restart```
    13a. Disconnect from the server.
    
  14. Reconnect now as grader with the key using: ```ssh -i ~/.ssh/udacity_key.rsa grader@35.182.93.218```
  
  15. Now edit the sshd_config file with: ```sudo nano /etc/ssh/sshd_config```
    15a. Change the *PasswordAuthentication* line to no if it's not already.
    15b. Change the ssh port from 22 to 2200, which you can find at the top of the file.
    15c. After saving, restart the ssh service again.
    
  16. Now reconnect like last time, but specify the port 2200 with: ```ssh -i ~/.ssh/udacity_key.rsa -p 2200 grader@35.182.93.218```

  17. Now we can disable ssh login for root user by editing sshd_config again.
    17a. Change *PermitRootLogin* to ```no```.
    17b. Restart ssh service again.
    
  18. Lastly configure and start the firewall.
    18a. ```sudo ufw allow 2200/tcp```
    18b. ```sudo ufw allow 80/tcp```
    18c. ```sudo ufw allow 123/udp```
    18d. ```sudo ufw enable``` (Yes to confirm)

# Deploy Catalog app to server

At this point everything should be set up, we just need to put our already working catalog application onto our newly created server.

  1. Install required packages
    1a. ```sudo apt-get install apache2```
    1b. ```sudo apt-get install libapache2-mod-wsgi python-dev```
    1c. ```sudo apt-get install git``` (Mine was already installed, but just in case)
  
  2. Enable mod_wsgi with: ```sudo a2enmod wsgi```
    2a. Start web server with: ```sudo service apache2 start```
    2b. Apache2 Ubuntu Default Page should now load. (Which it did for me)
    
  3. Create folder structure to store your project files in.
    3a. ```cd /var/www```
    3b. ```sudo mkdir catalog```
    3c. ```sudo chown -R grader:grader catalog```
    3d. ```cd catalog```
    
  4. Then clone in your project from your catalog project repo: ```git clone [repo link] catalog```
  
  5. Create a *catalog.wsgi* file and edit it with nano.
    5a. Type: ```
    import sys
    import logging
    logging.basicConfig(stream=sys.stderr)
    sys.path.insert(0, "/var/www/catalog/")

    from catalog import app as application
    application.secret_key = 'SeCrEtKey'
    ``` (Set secret_key to whatever your catalog projects secret_key is)
    
 6. Rename the *project.py* file to *__init__.py*
 
 7. Next, we install the virtual machine!
  7a. ```sudo pip install virtualenv```
  7b. ```sudo virtualenv venv```
  7c. ```source venv/bin/activate```
  7d. ```sudo chmod -R 777 venv``` (At this point you should see a **(venv)** in your command line.
  
  8. Install all requirements  to run the catalog project
    8a. The requirements can be found listed in the *README.md* file.
   
  9. Chanage the open('client_secrets.json', 'r') in the __init__.py file to:
    ```open('/var/www/catalog/catalog/client_secrets.json', 'r')```
    9a. At this point I also added the host IP to the client_secrets.json *javascript origins* and *redirect uris*
    9b. While you're in the __init__.py file, change the ```app.run()``` code to your host ip, and port number 80.
    
  10. Now create a virtual host config file with: ```sudo nano /etc/apache2/sites-available/catalog.conf```
    10a. Paste the code below ```
      <VirtualHost *:80>
        ServerName [YOUR PUBLIC IP ADDRESS]
        ServerAlias [YOUR AMAZON LIGHTSAIL HOST NAME]
        ServerAdmin admin@35.167.27.204
        WSGIDaemonProcess catalog python-path=/var/www/catalog:/var/www/catalog/venv/lib/python2.7/site-packages
        WSGIProcessGroup catalog
        WSGIScriptAlias / /var/www/catalog/catalog.wsgi
        <Directory /var/www/catalog/catalog/>
            Order allow,deny
            Allow from all
        </Directory>
        Alias /static /var/www/catalog/catalog/static
        <Directory /var/www/catalog/catalog/static/>
            Order allow,deny
            Allow from all
        </Directory>
        ErrorLog ${APACHE_LOG_DIR}/error.log
        LogLevel warn
        CustomLog ${APACHE_LOG_DIR}/access.log combined
      </VirtualHost>
      ``` (Replace pub ip and host name with YOUR ip and hostname)
      
    11. Now setup the database for your catalog
      11a. ```sudo apt-get install libpq-dev python-dev```
      11b. ```sudo apt-get install postgresql postgresql-contrib```
      11c. ```sudo -i -u postgres``` (After this you should see yourself as user postgres in the command line)
      11d. ```CREATE USER catalog WITH PASSWORD '123456';```
      11e. ```ALTER USER catalog CREATEDB;```
      11f. ```CREATE DATABASE catalog WITH OWNER catalog;```
      11g. ```\c catalog```
      11h. ```REVOKE ALL ON SCHEMA public FROM public;```
      11i. ```GRANT ALL ON SCHEMA public TO catalog;```
      11j. After that, quit the postgresql command line with ```\q``` and exit.
      
    12. Now go through each file in your project and change all engine creation points where it says
        ```'sqlite:///dbname.db'``` to ```'postgresql://catalog:123456@localhost/catalog'```
        
    13. After that use *database_setup.py* to generate the new db file. (You can use \dt in postgres to make sure it's created properly)
    
    14. Restart Apache server service one last time, and at this point your site should load!
        
# References

These are the guides that I followed, and unfortunately I had no luck:

- [cuanqin3's guide](https://github.com/chuanqin3/udacity-linux-configuration)
- [rrjoson's guide](https://github.com/rrjoson/udacity-linux-server-configuration/blob/master/README.md)
- [stueken's guide](https://github.com/stueken/FSND-P5_Linux-Server-Configuration)
