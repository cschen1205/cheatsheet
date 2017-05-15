Link: http://centoshowtos.org/monitoring/graphite/

# Detailed Steps #


Graphite is a scalable real-time metrics graphing system, that is sometimes used in lieu of other systems like cacti, munin, collectd, or zabbix.

Getting the software installed on CentOS is fairly straightforward with RPM packages available in the EPEL repository.

Make sure you have the python and gcc installed: 

> sudo yum install python
> sudo yum group install "Development tools"

Install graphite software and MySQL for backend

> sudo yum install cairo cairo-devel
> sudo yum install python-pip
> sudo pip install carbon
> sudo pip install whisper
> sudo pip install django==1.6.11
> sudo pip install django-tagging==0.3.1
> sudo pip install graphite-web
> sudo pip install cairocffi
> sudo yum install python-twisted-core
> sudo yum install MySQL-python

Install and start MySQL, lock it down, and create a Graphite database and user.

> sudo yum install mariadb mariadb-server
> sudo systemctl enable mariadb.service
> sudo systemctl start mariadb.service
> sudo mysql_secure_installation

# Create graphite db, username and password
mysql -e "CREATE DATABASE graphite;" -u root -p
mysql -e "GRANT ALL PRIVILEGES ON graphite.* TO 'graphite'@'localhost' IDENTIFIED BY 'my@123.PS';" -u root -p
mysql -e 'FLUSH PRIVILEGES;' -u root -p

# Configure Graphite to talk to this DB

The default install of graphite uses sqlite as a backend database, which is fine for testing purposes, but we’re going to use the MySQL database setup earlier.



Next, edit the local_settings.py file for graphite.

sudo cp /opt/graphite/webapp/graphite/local_settings.py.example /opt/graphite/webapp/graphite/local_settings.py

vi /opt/graphite/webapp/graphite/local_settings.py

Change the secrete key:

SECRET_KEY = 'my@123.PS'

Change the authentication to true:

ALLOW_HOSTS = [ '*' ]
USE_REMOTE_USER_AUTHENTICATION = True


Change the database configuration to look like this:

DATABASES = {
 'default': {
 'NAME': 'graphite',
 'ENGINE': 'django.db.backends.mysql',
 'USER': 'graphite',
 'PASSWORD': 'my@123.PS',
 'HOST': 'localhost',
 'PORT': '3306'
 }
}

Change the timezone

TIME_ZONE='Asia/Singapore'

Run the manage script for graphite to create the MySQL tables

> cd /opt/graphite/webapp/graphite
> sudo python manage.py syncdb

if syncdb cause command not found, then run (for new version) 

> sudo python manage.py migrate

It will prompt you to create a superuser, you can go ahead and do so.

superuser: 
  -name: my
  -password: my@123.PS
  -email: my.jenkins@gmail.com
  

Put in some default configs for carbon (which is the storage backends of graphite):

> cd /opt/graphite/conf
> sudo cp storage-schemas.conf.example storage-schemas.conf
> sudo cp carbon.conf.example carbon.conf
> sudo cp aggregation-rules.conf.example aggregation-rules.conf

Append the following lines to the end of aggregation-rules.conf:

<domain>.<measurement>.<instance>.user.all (60) = sum <domain>.<measurement>.<instance>.user.*
<domain>.<measurement>.<instance>.<kpi>.user.all (60) = avg <domain>.<measurement>.<instance>.<kpi>.user.*

Run the carbon-cache and graphite

> cd /opt/graphite

Finally set two rules for sending and viewing data on graphite in azure:

> PORT: 2003 (intranet)
> PORT: 80 (internet)

To test run graphite:

> sudo bin/carbon-cache.py start
> sudo bin/carbon-aggregator.py start
> sudo nohup bin/run-graphite-devel-server.py --port 80 /opt/graphite/ &

Now go to the VM ip and you should see graphite running.

That’s it, you’ve now got graphite installed and running. Next step is to start gathering statistical data, install carbon:
 
take a look at the python sample codes:

> vi /opt/graphite/examples/example-client.py

