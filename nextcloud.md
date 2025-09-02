# Nextcloud Nginx Fpm Postgresql
install nginx \
install Fpm \
install fpm \
### php-fpm
https://docs.nextcloud.com/server/latest/admin_manual/installation/php_configuration.html#php-modules \
Required modules \
PHP module ctype  \
PHP module curl \
PHP module dom \
PHP module fileinfo (included with PHP) \
PHP module filter (only on Mageia and FreeBSD) \
PHP module GD \
PHP module hash (only on FreeBSD) \
PHP module JSON (included with PHP >= 8.0) \
PHP module libxml (Linux package libxml2 must be >=2.7.0) \
PHP module mbstring \
PHP module openssl (included with PHP >= 8.0) \
PHP module posix \
PHP module session \
PHP module SimpleXML \
PHP module XMLReader \
PHP module XMLWriter \
PHP module zip \
PHP module zlib \
PHP module pdo_pgsql (PostgreSQL) \
PHP module pgsql (PostgreSQL) \
More \
bz2 \
intl \
sodium \

gmp \
exif \
apcu \
memcache \ 

Check enabled modules : php-fpm -m
apt install php8.2-fpm \
apt install php8.2-{bcmath,fpm,xml,zip,intl,ldap,imap,imagick,gd,gpm,bz2,curl,mbstring,pgsql,opcache,soap,memcached,zip,smbclient,redis} \


echo "\
apc.enable_cli=1;
opcache.enable=1; 
opcache.interned_strings_buffer=32;
opcache.max_accelerated_files=10000; 
opcache.memory_consumption=128; 
opcache.save_comments=1; 
opcache.revalidate_freq=60; 
opcache.jit=1255; 
opcache.jit_buffer_size=128M;\
" > /etc/php/8.2/fpm/conf.d/nextcloud-php-ext-apcu.ini

echo "\
memory_limit=512M; 
upload_max_filesize=512M;
post_max_size=512M;\
"> /etc/php/8.2/fpm/conf.d/nextcloud.ini



### PostgreSQL
pg_hba.conf -> local   all             postgres                                trust

su - postgres \
createuser -d nextcloud \
psql -c "GRANT CREATE ON SCHEMA public TO nextcloud;" \
createdb -E UTF-8 -O nextcloud nextcloud

echo "\
[PostgresSQL]
pgsql.allow_persistent = On
pgsql.auto_reset_persistent = Off
pgsql.max_persistent = -1
pgsql.max_links = -1
pgsql.ignore_notice = 0
pgsql.log_notice = 0 \
" > /etc/php/8.2/fpm/conf.d/pgsql.ini


### download nextcloud latest source
curl -fsSL -o nextcloud.tar.bz2 https://download.nextcloud.com/server/releases/latest.tar.bz2 \
tar -xjf nextcloud.tar.bz2 -C /hdd/0/local/www/ \
cd /hdd/0/local/www/nextcloud \
mkdir data \
mkdir custom_apps

wget https://download.nextcloud.com/server/releases/latest.zip \
unzip latest.zip -d /opt/ 
