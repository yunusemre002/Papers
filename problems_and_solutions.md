# **İNFLUXDB SORGULARI İLE İLGİLİ BİR SORUN VARSA KESİNLİKLE TİME RANGE İLE ALAKALIDIR.**

* **Error: Cannot find module 'axios' :** VScode node js olan dizine git. terminali aç ve aşağıdaki kodu yaz.  
`npm install axios --save` Bunu ilk kurduğun bir yerde diyorsa go to do gitlab read.me (run: npm install on local pc then run docker-compose) çünkü bind edilince konteyner için oluşturulan module localde olmadığı için siliniyor.)

* **Medium da `böyle yazı` yazmak isteniyorsa** ilgili yazı seçilip çift tırnak öğesine tıklanarak code bloğu elde edilmiş olur.

* **Ubunutu update den sonra saçma saçma şeyler olduysa** ekran ve pencerelerde değiştirmek için 
`sudo apt install gnome-tweaks` ile tweaks indir ve bunun üzeirnde düzenlemeler ile istenilen hale getirilebilir.

* **.gitignore doesn't work** 
İf ".gitignore" doesn't work, run do below comment then it will run.  
`git rm -rf --cached .`
`git add .`


## Ubuntu'ya docker kurulurken
  ```
  yed@YEDPC:~$  sudo apt-get install docker-ce docker-ce-cli containerd.io
  Reading package lists... Done
  Building dependency tree       
  Reading state information... Done
  Package docker-ce is not available, but is referred to by another package.
  This may mean that the package is missing, has been obsoleted, or
  is only available from another source

  E: Package 'docker-ce' has no installation candidate
  E: Unable to locate package docker-ce-cli
  E: Unable to locate package containerd.io
  E: Couldn't find any package by glob 'containerd.io'
  ```
  sadece `sudo apt-get install containerd.io` yazman yeterli değil :) .
  https://unix.stackexchange.com/questions/363048/unable-to-locate-package-docker-ce-on-a-64bit-ubuntu 
  ```
  sudo apt install apt-transport-https ca-certificates curl software-properties-common
  curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
  sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu focal stable"
  ```

* <s>böyle yazmak için</s> `<s> ... </s>` yapsıı kullanılır.

## **Linux bilgisayarında wired and wifi varken internet bağlantısının kopması**  
  https://askubuntu.com/questions/263572/can-i-use-wi-fi-and-cable-ethernet-networking-at-same-time 

  ![wifi_connection_lost_0](https://github.com/yunusemre002/Papers/blob/main/photos/wifi_connection_lost_0.png?raw=true)  
  Gateway bilgisini boş bırak  
  ![wifi_connection_lost_1](https://github.com/yunusemre002/Papers/blob/main/photos/wifi_connection_lost_1.png?raw=true)  
  "Use this connection only for resources on its network" bilgisini işaretle.  
  ![wifi_connection_lost_2](https://github.com/yunusemre002/Papers/blob/main/photos/wifi_connection_lost_2.png?raw=true)  
  ![wifi_connection_lost_3](https://github.com/yunusemre002/Papers/blob/main/photos/wifi_connection_lost_3.png?raw=true)  

## Postgres backup:  
  https://www.postgresqltutorial.com/postgresql-backup-database/
  `pg_dump -U username -W -F t database_name > c:\backup_file.tar`  
  https://stackoverflow.com/questions/2732474/restore-a-postgres-backup-file-using-the-command-line  
  `pg_restore --clean -U digitalfactory -d digitalfactory -1 backup_file.tar`
  
## Docker Network on Windows
   Docker'ı windows üzerinde `network_mode = Host` olarak kullanamaz. Bu sebeple bridge mod olarak kullanılır. Bridge mod da kullanırken aynı networkte bir çok container barınabilir. Bu containerler birbirleriyle haberleşirken `http://container_name:default_port_address` olarak konuşmalılardır. Docker, container_name kısmını ip'ye çevirip öyle kullanır. Ayrıca oraya ip yazlırsa docker her restart olduğunda tüm ip ler değişeceği için yeniden ayarlama yapmak gerkecektir. Örneğin DF isimli bir netwokümüzde 3 adet container olsun isimleri: influxdb, grafana ve nodered. Nodered'ten influxdb'ye bağlanmak isteniliyorsa `http://influxdb:8086` yazılarak bağlanması sağlanabilir. influxdbnin port 8090:8086 olarak belirlenmeiş olsa dahi aynı networkten erişim sağlanırken 8086 portuna istek gönderilir.

## Remove SSH password question
**Local:**  
- `ssh-keygen -t rsa`  
- `ssh-copy-id root@192.168.1.55` (192.168.1.55 is remote pc ip) enter password...  
- it has been finished. Anymore it can be entered without password.  
- `ssh root@192.168.88.235`  

# Forticlient cannot install into Ubuntu 20.04
Alternatif olarak [ssl_vpn](https://hadler.me/linux/forticlient-sslvpn-deb-packages/ ) kullanılabilir.
Watch this [link](https://www.youtube.com/watch?v=0RodMfQHoLA&ab_channel=veTechno)

# Ubuntu bozuldu ekran gelmiyor. BIOS ekranında kalıyor. Ama yazı yazamıyorsan
`ctrl alt f1` || `ctrl alt f2` || `ctrl alt f3`
I experienced the same problem; though Mark's (top answer) solution didn't immediately work (since ctrl+alt+F2 etc. brings up a flickering TTYL which is nearly impossible to use), his suggested cause was correct, as it was a problem with the nvidia [driver.](https://askubuntu.com/questions/882385/dev-sda1-clean-this-message-appears-after-i-startup-my-laptop-then-it-w )
`sudo apt-get purge nvidia*`
`sudo ubuntu-drivers autoinstall` #Make sure you have internet connection
https://askubuntu.com/questions/951046/unable-to-install-nvidia-drivers-unable-to-locate-package

# Ubuntu 20.04 No Wifi Adapter Found. Lenovo Ideapad Realtek
To get the realtek 8852 PCI adapter working with Ubuntu, follow these [steps:](https://askubuntu.com/questions/1352731/ubuntu-20-04-no-wifi-adapter-found-lenovo-ideapad-realtek)

Download dependencies:
```
sudo apt-get update
sudo apt-get install make gcc linux-headers-$(uname -r) build-essential git
```  
Install 'fix':
```
git clone https://github.com/lwfinger/rtw89.git -b v5
cd rtw89 && make && sudo make install
``` 
load module:
```
sudo modprobe rtw89pci
```  

