# Moodle installation on Debian 12 

### Locales
dpkg-reconfigure locales

## Nginx Php
# https://docs.moodle.org/403/en/Nginx

### Install Php 

#sudo apt install  php8.-cli php8.1-fpm -y \
sudo apt install  php8.2-cli php-fpm -y \
sudo apt install  php8.2-pgsql php8.2-gd php8.2-mbstring php8.2-xmlrpc php8.2-xml  php8.2-curl php8.2-zip php8.2-intl   php8.2-soap -y \
nano /etc/php/8.2/fpm/pool.d/www.conf -> uncomment security.limit_extensions =
**/etc/php/8.1/**

nano /etc/php/8.2/fpm/php.ini \
change \
memory_limit = 512M \
cgi.fix_pathinfo = 0 \
upload_max_filesize = 128M \
max_execution_time = 360 \
date.timezone = Asia/Ho_Chi_Minh

### Git
cd /var/www
git clone git://git.moodle.org/moodle.git ./moodle \

git branch -a \
git branch --track MOODLE_403_STABLE origin/MOODLE_403_STABLE     (3)
git checkout MOODLE_403_STABLE                                   (4)

###
cd /var/www \
wget https://download.moodle.org/download.php/direct/stable403/moodle-4.3.1.tgz    \
tar -zxf moodle-4.3.1.tgz \
chown -R www-data:www-data \
mkdir /var/www/moodledata \
chown www-data:www-data /var/www/html/moodledata \
chmod 777 /var/www/moodledata \
chmod -R 755 /var/www/html/moodle \


### Postgresql

su - postgres \
CREATE USER moodle; \
CREATE DATABASE moodle WITH OWNER moodle ENCODING 'UTF8'; \

### nginx

### ssl certificate
certbot --nginx -v

