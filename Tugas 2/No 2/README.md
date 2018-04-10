## Soal 1

#### Round Robin:
Algoritma ini akan membagi pengakses webiste akan menjadi sama rata. Algoritma ini sangat sederhana sehingga harus di konfigurasi lagi untuk masalah session agar tidak terjadi masalah

### Least Connection
Algortima ini akan membagi pengakses kepada yang memiliki beban yang rendah. Tetapi akan memiliki masalah yang sama dengan Round Robin untuk sessionnya

### IP Hast
Algoritma ini algoritma yang akan membagi berdasarkan ditribusi IPnya. Sehingga 1 IP akan mengakses 1 server yang sama terus menerus sampai terjadi masalah misalnya server mati atau down akan di pindahkan ke server lain.