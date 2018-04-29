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
