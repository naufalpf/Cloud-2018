# Tugas 2
Buatlah sistem load balancing dengan 1 load balancer(nginx dan 2 worker(apache2), terapkan algoritma load balancing round-robin, least-connected, dan ip-hash.

## Soal :
1. Buatlah Vagrantfile sekaligus provisioning-nya untuk menyelesaikan kasus.
2. Analisa apa perbedaan antara ketiga algoritma tersebut.
3. Biasanya pada saat membuat website, data user yang sedang login disimpan pada session. Sesision secara default tersimpan pada memory pada sebuah host. Bagaimana cara mengatasi masalah session ketika kita melakukan load balancing?

## Solusi: 
## Soal 1

### Langkah :
1. Membuat Vagrant Box
```
Vagrant init
```
2. Mengganti konfigurasi dan membuat file provsion
```
config.vm.box = "ubuntu/xenial64"
config.vm.network "private_network", ip: "192.168.0.2"
config.vm.provision "shell", path:'hai.sh'
```
3. Install Nginx dan php 7 menggunakan Provision
```
sudo apt-get update
sudo apt-get install -y nginx
sudo apt-get install -y php7.0 php7.0-fpm php7.0-cli php7.0-common php7.0-mbstring php7.0-gd php7.0-intl php7.0-xml php7.0-mysql php7.0-mcrypt php7.0-zip
```
4. Setting load balancer di **/etc/nginx/site-available/default**
```
server {
        listen   8000 default_server;
        listen   [::]:8000 ipv6only=on default_server;
        server_name _;
        }
```

5. Membuat load banlancer
	1. Load Balancer Biasa
```
upstream worker
{
        server 202.46.129.90:80;
        server 103.94.189.5;
}
location / 
{
	proxy_pass http://worker;
}
```
	2. Untuk load balancer Round Robin
			upstream worker
			{
        	leat_conn;
            server 202.46.129.90:80;
        	server 103.94.189.5;
			}
			location / 
            {
            proxy_pass http://worker;
			}
	3. Untuk load balancer IP Hash
    ```
    upstream worker
			{
        	ip_hash;
            server 202.46.129.90:80;
        	server 103.94.189.5;
			}
			location / 
            {
            proxy_pass http://worker;
			}
    ```
6. Konfigurasi mengaktifkan dengan perintah **sudo ln -s /etc/nginx/sites-available/default /etc/nginx/site-enable/**
config.php
		
		# Add index.php to the list if you are using PHP
        index index.php index.html index.htm index.nginx-debian.html;

        # pass the PHP scripts to FastCGI server listening on "lb"
		location ~ \.php$ {
               include snippets/fastcgi-php.conf;
               fastcgi_pass lb;
        }

7. Restart Nginx
		
		sudo service nginx restart

8. Edit file config PHP-fpm pada Worker 1 dan 2

		/etc/php/7.0/fpm/pool.d/www.conf

Ubah variabel
		
		dari :
			listen = /run/php/php7.0-fpm.sock
		
		menjadi :
			listen = 9000

9. Restart PHP-fpm

		sudo service php7.0-fpm restart

10.  Buat file PHP di masing masing Load balancer dan Worker

		/var/www/html/index.php
		
## Soal 2

#### Round Robin:
Algoritma ini akan membagi pengakses webiste akan menjadi sama rata. Algoritma ini sangat sederhana sehingga harus di konfigurasi lagi untuk masalah session agar tidak terjadi masalah

### Least Connection
Algortima ini akan membagi pengakses kepada yang memiliki beban yang rendah. Tetapi akan memiliki masalah yang sama dengan Round Robin untuk sessionnya

### IP Hast
Algoritma ini algoritma yang akan membagi berdasarkan ditribusi IPnya. Sehingga 1 IP akan mengakses 1 server yang sama terus menerus sampai terjadi masalah misalnya server mati atau down akan di pindahkan ke server lain.

## Soal 3

### Session Affinity (Sticky Sessions)
Adalah Sebuah metode yang digunakan untuk aplikasi load balancing. Router atau load balancer dapat menetapkan satu server ke pengguna tertentu, berdasarkan sesi HTTP atau alamat IP mereka. Server yang ditetapkan diingat oleh router untuk jangka waktu tertentu, memastikan bahwa semua permintaan masa depan untuk sesi yang sama dikirim ke server yang sama.

## Jawaban
[No 1](https://github.com/ariya01/Cloud/tree/master/Nginx/No%201)
[No 2](https://github.com/ariya01/Cloud/tree/master/Nginx/No%202)
[No 3](https://github.com/ariya01/Cloud/tree/master/Nginx/No%203)
