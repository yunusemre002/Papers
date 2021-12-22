
* `ls -lh`	the apparent space of the directory and true space of the files and in details the names, creation date of each.  
* `ls -s` 	to list file size,  
* `ls -sh` 	for human readable sizes.  
* `du -h` 	for human readable sizes for directories  
  
* find needs a starting point, and the . (dot) points to the current directory.  
`find . -name "foo*"`

* To install a .deb file by opening a terminal and typing:
  `sudo dpkg -i package_file.deb`
### Dosya izinleri [Bkz.](https://www.yusufsezer.com.tr/linux-dosya-ve-dizin-izinleri/)
* `chmod 777 file_name` Herkese (-o, -g, -u) tüm izinleri (-r, -w, -x) verir. Eğer bir directory ise ve alt file lar için de izin isteniyorsa `chmod -R 777 fine_name` denilmeli. Bunun için güzel dökümanlar va r neden 7 neden user belirtilmiyor vs.
* Tüm kullanıcılara sadece çalıştırma izni verilmesi için `chmod +x file_name`
* `chown yunus file` --> Changes the ownership of the file from its current owner to user bob.

```
yed@yed:~/Desktop$ cal 
     Kasım 2021       
Pa Pz Sa Çr Pr Cu Ct  
    1  2  3  4  5  6  
 7  8  9 10 11 12 13  
14 15 16 17 18 19 20  
21 22 23 24 25 26 27  
28 29 30                
                   
yed@yed:~/Desktop$ cal 1453

# tüm takvimi verir
```
* `top`
* `pgrep teamviewed`
* `pkill teamviewed`
* `service teamviewed restart`
* `service <servis_adi> status` ex: `service teamviewed status` 
```
● teamviewerd.service - TeamViewer remote control daemon
     Loaded: loaded (/etc/systemd/system/teamviewerd.service; enabled; vendor preset: enabled)
     Active: active (running) since Fri 2021-11-19 17:04:43 +03; 35s ago
    Process: 40872 ExecStart=/opt/teamviewer/tv_bin/teamviewerd -d (code=exited, status=0/SUCCESS)
   Main PID: 40874 (teamviewerd)
      Tasks: 30 (limit: 17713)
     Memory: 5.5M
     CGroup: /system.slice/teamviewerd.service
             └─40874 /opt/teamviewer/tv_bin/teamviewerd -d

Kas 19 17:04:43 yed systemd[1]: Starting TeamViewer remote control daemon...
Kas 19 17:04:43 yed systemd[1]: teamviewerd.service: Can't open PID file /run/teamviewerd.pid (yet?)>
Kas 19 17:04:43 yed systemd[1]: Started TeamViewer remote control daemon.

```

`history` derseniz geçmiş komutlara bakılabilir.


### `nano file` 
* **Ctrl+K :**  Cut
* **Alt+6  :**	Copy
* **Ctrl+U:**	Paste
* **Alt+3 :**	Comment/uncomment
* **Alt+U :**	Undo
* **Alt+E :**	Redo
* **Alt+Del**	Delete current line

### Shortcuts
* **CTRL+U** Terminale yazılan şeyleri siler.
* **CTRL+A** İmleci terminale yazılan değerin başına getirir.
* **CTRL+E** İmleci terminale yazılan değerin sonuna getirir.
* **CTRL+L** Terminal ekranını temizler
  
# Kullanıcı işlemleri
`cat /etc/passwd` kullanıcı listesini tututyor.
`ls -l /home` var olan kullanıcı isimlerini görebilirsiniz.  
`sudo passwd yed` taner kullanıcısının şifresinin değiştir.  
`su yed` switch user ile yed kullanıcısına geçilir.  
`sudo deluser yed` sil.  
Ama kullanıcı directory'si silinmez. İçinde önemli bişiler olabilir idiye.BU dizin /home altında bulunur. Bu dizini silmek için aşağıdaki komut kullanılır.
`sudo rm -rf yed` -r dizin sil içindekilerle birlikte f force et sorular sorarsa yes de.

  

### Kaynaklar:
* [nano](https://www.nano-editor.org/dist/latest/cheatsheet.html)
