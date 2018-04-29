# Tugas Komputasi Awan 2018

## Kelompok
1. Naufal Pranasetyo F.		5115100057
2. Ariya Wildan Devanto		5115100123

## Link Laporan

Tugas 1: [Vagrant](/no.1)

# Tugas no.1

# Untuk menambahkan User di VAGRANT

### * Berhasil
#### Ditambah dengan
Dengan Perintah ini:
```
echo -e "buayakecil\nbuayakecil\n" | sudo adduser --gecos "" awan
```
![1](https://github.com/ariya01/Cloud/blob/master/Vagrant/no.1/gambar/Screenshot%20from%202018-03-13%2003-21-01.png)
#### Dan Berhasil
![2](https://github.com/ariya01/Cloud/blob/master/Vagrant/no.1/gambar/Screenshot%20from%202018-03-13%2003-21-13.png)
### * Kegagalan 
#### Ditambahkan User dan Password
Dengan Perintah ini:
```
Vagrant.configure("2") do |config|
  config.ssh.username = "user"
  config.ssh.password = "password"
end
```
![3](https://github.com/ariya01/Cloud/blob/master/Vagrant/no.1/gambar/Screenshot%20from%202018-03-13%2003-06-43.png)
#### Tetapi gagal untuk auth
![4](https://github.com/ariya01/Cloud/blob/master/Vagrant/no.1/gambar/Screenshot%20from%202018-03-13%2003-06-27.png)

# no.2

## Installing FrameWork PHOENIX
Dengan Perintah ini:
```
wget https://packages.erlang-solutions.com/erlang-solutions_1.0_all.deb && sudo dpkg -i erlang-solutions_1.0_all.deb
sudo apt-get update
sudo apt-get install elixir esl-erlang -y

sudo update-locale LC_ALL=en_US.UTF-8

yes | mix local.hex

yes | mix archive.install https://github.com/phoenixframework/archives/raw/master/phx_new.ez

sudo apt-get install inotify-tools -y
```
![alt text](https://github.com/ariya01/Cloud/blob/master/Vagrant/no.2/gambar/Screenshot%20from%202018-03-13%2004-58-31.png)

## Check 
Dengan Perintah ini:
```
mix phoenix.new chatter
```

![alt text](https://github.com/ariya01/Cloud/blob/master/Vagrant/no.2/gambar/Screenshot%20from%202018-03-13%2004-57-37.png)

# 3. Buat vagrant virtualbox dan lakukan provisioning install:

    php
    mysql
    composer
    nginx

setelah melakukan provioning, clone https://github.com/fathoniadi/pelatihan-laravel.git pada folder yang sama dengan vagrantfile di komputer host. Setelah itu sinkronisasi folder pelatihan-laravel host ke vagrant ke /var/www/web dan jangan lupa install vendor laravel agar dapat dijalankan. Setelah itu setting root document nginx ke /var/www/web. webserver VM harus dapat diakses pada port 8080 komputer host dan mysql pada vm dapat diakses pada port 6969 komputer host

# Jawaban no. 3
config vm.box menggunakan Ubuntu 16.04
dan forward port
![alt text](https://github.com/ariya01/Cloud/blob/master/Vagrant/no.3/nomer41.png)

Lakukan sinkronisasi folder
dengan perintah ini:
```

config.vm.synced_folder "pelatihan-laravel/", "/var/www/web", id: "vagrant-root",
    owner: "vagrant",
    group: "www-data",
    mount_options: ["dmode=775,fmode=664"]

```

## Script Provisioning
Kami membuat file laravel.sh dan tambahkan script-script dibawah pada file tersebut.

### Script untuk install PHP7
```

add-apt-repository ppa:ondrej/php
apt-get update
apt-get install -y python-software-properties software-properties-common

apt-get install -y php7.1 php7.1-fpm
apt-get install -y php7.1-mysql
apt-get install -y mcrypt php7.1-mcrypt
apt-get install -y php7.1-cli php7.1-curl php7.1-mbstring php7.1-xml php7.1-mysql
apt-get install -y php7.1-json php7.1-cgi php7.1-gd php-imagick php7.1-bz2 php7.1-zip
```

### Script untuk install mysql
```
debconf-set-selections <<< 'mysql-server mysql-server/root_password password vagrant'
debconf-set-selections <<< 'mysql-server mysql-server/root_password_again password vagrant'
sudo apt-get -y update
apt-get install -y mysql-server
apt-get install -y mysql-client
```

### Install Nginx
```
sudo apt-get -y update
sudo apt-get -y install nginx
```
## Clone file laravel
```
git clone https://github.com/fathoniadi/pelatihan-laravel.git
cd pelatihan-laravel
cp .env-example .env
sudo composer update
php artisan key:generate
php artisan migrate
php artisan db:seed
```

### Edit konfigurasi nginx di /etc/nginx/site-available/default
```
sudo nano /etc/nginx/site-available/default
```
seperti konfig ini:
```
echo '' > /etc/nginx/sites-available/default
cat >> /etc/nginx/sites-available/default <<'EOF'
server {
	# Server listening port
	listen 80;

	# Server domain or IP
	server_name localhost;

	# Root and index files
	root /var/www/web/public;
	index index.php index.html index.htm;	

	# Urls to attempt
	location / {
                try_files $uri $uri/ /index.php?$query_string;
        }

	# Configure PHP FPM
	location ~* \.php$ {
		fastcgi_pass unix:/var/run/php/php7.1-fpm.sock;
		fastcgi_index index.php;
		fastcgi_split_path_info ^(.+\.php)(.*)$;
		fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
		include /etc/nginx/fastcgi_params;
	}

	# Debugging
	access_log /var/log/nginx/localhost_access.log;
	error_log /var/log/nginx/localhost_error.log;
	rewrite_log on;
}
EOF

```

### Edit konfigurasi file mysql di /etc/mysql/my.cnf
```
sudo nano /etc/mysql/my.cnf
```
```
#
# The MySQL database server configuration file.
#
# You can copy this to one of:
# - "/etc/mysql/my.cnf" to set global options,
# - "~/.my.cnf" to set user-specific options.
# 
# One can use all long options that the program supports.
# Run program with --help to get a list of available options and with
# --print-defaults to see which it would actually understand and use.
#
# For explanations see
# http://dev.mysql.com/doc/mysql/en/server-system-variables.html

# This will be passed to all mysql clients
# It has been reported that passwords should be enclosed with ticks/quotes
# escpecially if they contain "#" chars...
# Remember to edit /etc/mysql/debian.cnf when changing the socket location.

# Here is entries for some specific programs
# The following values assume you have at least 32M ram

[mysqld_safe]
socket		= /var/run/mysqld/mysqld.sock
nice		= 0

[mysqld]
#
# * Basic Settings
#
user		= mysql
pid-file	= /var/run/mysqld/mysqld.pid
socket		= /var/run/mysqld/mysqld.sock
port		= 3306
basedir		= /usr
datadir		= /var/lib/mysql
tmpdir		= /tmp
lc-messages-dir	= /usr/share/mysql
# skip-external-locking
#
# Instead of skip-networking the default is now to listen only on
# localhost which is more compatible and is not less secure.
bind-address	= 0.0.0.0
#
# * Fine Tuning
#
key_buffer_size		= 16M
max_allowed_packet	= 16M
thread_stack		= 192K
thread_cache_size       = 8
# This replaces the startup script and checks MyISAM tables if needed
# the first time they are touched
myisam-recover-options  = BACKUP
#max_connections        = 100
#table_cache            = 64
#thread_concurrency     = 10
#
# * Query Cache Configuration
#
query_cache_limit	= 1M
query_cache_size        = 16M
#
# * Logging and Replication
#
# Both location gets rotated by the cronjob.
# Be aware that this log type is a performance killer.
# As of 5.1 you can enable the log at runtime!
#general_log_file        = /var/log/mysql/mysql.log
#general_log             = 1
#
# Error log - should be very few entries.
#
log_error = /var/log/mysql/error.log
#
# Here you can see queries with especially long duration
#log_slow_queries	= /var/log/mysql/mysql-slow.log
#long_query_time = 2
#log-queries-not-using-indexes
#
# The following can be used as easy to replay backup logs or for replication.
# note: if you are setting up a replication slave, see README.Debian about
#       other settings you may need to change.
#server-id		= 1
#log_bin			= /var/log/mysql/mysql-bin.log
expire_logs_days	= 10
max_binlog_size   = 100M
#binlog_do_db		= include_database_name
#binlog_ignore_db	= include_database_name
#
# * InnoDB
#
# InnoDB is enabled by default with a 10MB datafile in /var/lib/mysql/.
# Read the manual for more InnoDB related options. There are many!
#
# * Security Features
#
# Read the manual, too, if you want chroot!
# chroot = /var/lib/mysql/
#
# For generating SSL certificates I recommend the OpenSSL GUI "tinyca".
#
# ssl-ca=/etc/mysql/cacert.pem
# ssl-cert=/etc/mysql/server-cert.pem
# ssl-key=/etc/mysql/server-key.pem
```

### Restart nginx dan phpnya

## Laravel

```
cd /var/www/web
cp .env.example .env

php artisan key:generate

sed -i "s/DB_DATABASE=blog/DB_DATABASE=dbq/g" .env
sed -i "s/DB_USERNAME=root/DB_USERNAME=root/g" .env
sed -i "s/DB_PASSWORD=/DB_PASSWORD=password/g" .env

composer install

php artisan migrate

```

## Hasil Testing

Test server bisa diakses dengan membuka port 8080 
Dapat dilihat dengan membuka http://localhost:8080 pada browser host
![alt text](https://github.com/ariya01/Cloud/blob/master/Vagrant/no.3/nomer4.png)

# no.4

## Install bind9 squid3
Dengan Perintah ini:
```
sudo apt-get update
sudo apt-get install -y squid3
sudo apt-get install -y bind9
```
![alt text](https://github.com/ariya01/Cloud/blob/master/Vagrant/no.4/gambar/Screenshot%20from%202018-03-13%2007-09-09.png)

Hasilnya :
![alt text](https://github.com/ariya01/Cloud/blob/master/Vagrant/no.4/gambar/Screenshot%20from%202018-03-13%2007-09-37.png)




