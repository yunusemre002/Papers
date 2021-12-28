* **Data driven:** Veriye dayalı karar alınıyor olmak. 
* **İmage progessing:** Görüntü işleme
* **Cloud'dayız:** Sistemlerin bir kısmını buluta taşımak.
* **Codesys:** PLC programlama uygulaması dilden bağımsız tüm plc lerde çalışacak bir platorm.
* **Modbus:** PLC’ler arası haberleşme protokolü. Endüstriyel alanlar için tasarlanmış. Ücretsiz.
* **MES (Manufacturing Execution System):** Üretim Yönetim Sistemi. Fabrika düzeyinde üretimi yönetme ve izleme süreçlerini kontrol eder. MES, gerçek zamanlı olarak robot, makineler, sensörler, operatör panelleri ve işçilerden belirlenen aralıklarla tüm üretim bilgilerini alır. MES‘in genel amacı, imalat işlemlerinin etkin bir şekilde yürütülmesini sağlamak ve üretim çıktısını iyileştirmektir. Bu amaca, komple bir ürün yaşam döngüsü hakkında gerçek zamanlı ve doğru veriler izleyerek ve toplayarak ulaşılmaktadır. MES dört temel işlevi yerine getirir:
  * Üretim Sahasından Anlık-Eş zamanlı veri toplar (örneğin makine duruşları).
  * Verileri düzenleyip merkezi bir veri tabanında saklar.(ERP ile ileri geri entegre şekilde)
  * Planlama ve mühendislik büro gibi verilerin saklandığı diğer bilgi sistemlerinden kritik verileri de dâhil ederek tüm verilerin ağ üzerinden erişilir olmasını sağlar.
  * ERP’den iş emirlerini alıp düzenleyerek üretim birimlerine aktarır. Aynı zamanda vakalara eş zamanlı detaylı planlama ile tepki vererek verimliliği ve kaliteyi artırır.[1]
* **ERP (Enterprise Resource Planning):** Kurumsal/işletme kaynak planlaması, işletmelerde mal ve hizmet üretimi için gereken işgücü, makine, malzeme gibi kaynakların verimli bir şekilde kullanılmasını sağlayan bütünleşik yönetim sistemlerine verilen genel addır. 
* **SOAP (Simple Object Access Protocol):** Basit Nesne Erişim Protokolü,  Farklı dilde , farklı platformlar üzerinden çalışan uygulamaların birbirleriyle haberleşmesi için ilk olarak SOAP API (Simple Object Access Protocol) servisleri kullanılmaktaydı. SOAP servisler HTTP, SMTP protokolleri üzerinden aldığı istekleri genellikle XML formatında dönüş yapar.
* **API (Application Programming Interface):** https://caylakyazilimci.com/post/api-nedir ...Günümüzde ise yaygın olarak REST API (Representational State Transfer) olarak da bilinen bir mimari yaklaşım tercih edilmektedir. 
* **Rest (REpresentational State Transfer) Api:** https://caylakyazilimci.com/post/restful-api-nedir-ve-standartlari-nelerdir  Rest de temelinde HTTP protokolü üzerinden haberleşen bir mimari yaklaşımdır. Bu mimari yaklaşımı kullanan API'lara da Restful API olarak adlandırıyoruz. Restful standardı daha eski ve katı kuralları olan SOAP servislere bir çözüm olarak geliştirilmiş ve tercih edilmiştir.  Restful olarak geliştirilmiş bir API üzerinden genellikle JSON tipinde dönüş yapılması tercih edilir fakat amaca ve isteğe göre XML, CSV, HTML veya düz bir metin olarak dönüş alabilirsiniz.
* **Endpoint:** API üzerinde belirli bir amaç için oluşturulmuş metodunuza verilen isim. 
* **JSON:** JSON günümüzde sunucular arasında haberleşme yapmak ve aynı zamanda ufak boyutta taşınabilmesinden ötürü oluşturulmuştur. XML standardına alternatif olarak oluşturulmuştur. Düzenli veri saklamak için de JSON formatında dosyalar oluşturulup burada anlamlı modeller oluşturulabilir. JSON içerisinde bir anahtar(key) ve o anahtara ait değer(value) içerir.
* **log rotate** Log rotation is the process that renames a current log file (e.g., auth.log becomes auth.log.1) and sets up a new log file (e.g., auth.log) for new log entries. Depending on the number of files to be retained, we might see something like logfile.6 becoming logfile.7 (with the old logfile.7 disappearing) and logfile.5 becoming logfile.6, etc. before the new logfile is created. The older log files might also be compressed, particuarly if they tend to be very large files. So, you might see logfile.1.gz instead of [logile.1.](https://www.networkworld.com/article/3218728/how-log-rotation-works-with-logrotate.html)
  ```
  root@yed:/home/yed# ls -lh /var/log/syslog*
  -rw-r----- 1 syslog adm 1,4M Ara 28 15:42 /var/log/syslog
  -rw-r----- 1 syslog adm 3,0M Ara 28 09:04 /var/log/syslog.1
  -rw-r----- 1 syslog adm 128K Ara 24 09:20 /var/log/syslog.2.gz
  -rw-r----- 1 syslog adm 134K Ara 23 09:17 /var/log/syslog.3.gz
  -rw-r----- 1 syslog adm 144K Ara 22 09:37 /var/log/syslog.4.gz
  -rw-r----- 1 syslog adm 137K Ara 21 09:36 /var/log/syslog.5.gz
  -rw-r----- 1 syslog adm 123K Ara 20 09:03 /var/log/syslog.6.gz
  -rw-r----- 1 syslog adm 143K Ara 17 09:32 /var/log/syslog.7.gz
  ```

## ??
* Optimum parametresi ile değerlerini ayarlamak. %1 bile düşürmek önemli... 
* Siber topraklarda veri işlemek
* Yapay Zekayı çekiç olarak görüp çivi aramaya çıkılmasın; Önce çivi bulunsun.



[1][https://trexakademi.com/mes-sistemlerinde-bakim-yonetimi-uygulamalari/]
