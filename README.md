# MySQL Monitoring Stack
mysql(local host) -> mysql exporter(on container) -> prometheus(on container) -> grafana(on container)

# create the user and password credentials for mysql
# change the username and password as you need

CREATE USER 'user'@'%' IDENTIFIED BY 'password';
GRANT PROCESS, REPLICATION CLIENT, SELECT ON *.* TO 'user'@'%';
FLUSH PRIVILEGES;

# Ensure MySQL bind-address = 0.0.0.0
if not,
sudo nano /etc/mysql/mysql.conf.d/mysqld.cnf
find this line  " bind-address = 127.0.0.1 "
change it to "bind-address = 0.0.0.0 "

restart mysql after changing the bind address

# file permission
chmod 644 .my.cnf

# change the mysql credentials in .my.cnf
usename=yourusername
password=yourpassword

# before starting the containers check the volumes mounted properly in the yml file
# check the entered ports in yml file that are availabe and free

## Start the containers

docker compose -f mysql_monitor.yml up -d

# check all the containers are running
 
# check the each port one by one

mysql-exporter - http://localhost:9104/
prometheus - http://localhost:9091/
grafana - http://localhost:3000/

# after verifing all ports
 login in grafana with credentials entered in yml file
 add the prometheus data source by creating new connection
 add new connection -> search and select - prometheus
 enter the URL of docker prometheus at connection - http://prometheus:9090
 save

 create new dashboard
 add visualization
 select the metric and run query 
 save the dashboard


 # successfully created a mysql monitor using prometheus and grafana
