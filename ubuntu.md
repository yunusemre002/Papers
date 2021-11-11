# **Ubuntu 20.04**

### 1- Teamviewer Kurulumu:  
```
wget https://download.teamviewer.com/download/linux/teamviewer_amd64.deb
sudo apt install ./teamviewer_amd64.deb
```
**Teamviewer ayarları:** 
1. Start teamviewer with system
    ![Start with system](https://github.com/yunusemre002/Papers/blob/main/photos/teamviewer_start_with_system.png?raw=true)  
      
3. Disable random password generator, set dtatic password
    ![Start with system](https://github.com/yunusemre002/Papers/blob/main/photos/teamviewer_set_static_password.png?raw=true)  

### 2- Chrome:
```
wget https://dl.google.com/linux/direct/google-chrome-stable_current_amd64.deb  
sudo apt install ./google-chrome-stable_current_amd64.deb  
```

### 3- Docker:
```
sudo apt install docker.io
sudo apt install apt-transport-https ca-certificates curl software-properties-common
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu focal stable"
```


### 4- NodeJS 14:
```
curl -sL https://deb.nodesource.com/setup_14.x | sudo bash -
sudo apt -y install nodejs
```

  
    
      
      






---------------------------------------------------------------------------------
[1- Teamviewer](https://linuxize.com/post/how-to-install-teamviewer-on-ubuntu-18-04/)  
[2- Chrome](https://linuxize.com/post/how-to-install-google-chrome-web-browser-on-ubuntu-18-04/)    
[3- Docker](https://unix.stackexchange.com/questions/363048/unable-to-locate-package-docker-ce-on-a-64bit-ubuntu)  
[4- NodeJS:14](https://computingforgeeks.com/install-node-js-14-on-ubuntu-debian-linux/)
