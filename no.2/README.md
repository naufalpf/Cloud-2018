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
![alt text](https://github.com/ariya01/Cloud/blob/master/no.2/gambar/Screenshot%20from%202018-03-13%2004-58-31.png)

## Check 
Dengan Perintah ini:
```
mix phoenix.new chatter
```

![alt text](https://github.com/ariya01/Cloud/blob/master/no.2/gambar/Screenshot%20from%202018-03-13%2004-57-37.png)
