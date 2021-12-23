# Task
## Offset
**Offset ne zaman kullanılır?**
Eğer raw_data biraz gecikmeli geliyorsa task çalıştığında eksik veri gelebilir. Mesela raw_data 15 saniyede bir geliyor olsun.  
* Ham data: 00:00:05, 00:00:35, 00:00:50, 00:01:05 saniyelerinde (raw_data'ya) veri ekliyor olsun.
* Task ise: 00:00:00, 00:01:00, 00:02:00 saniyelerinde çalışıyor olsun.   

Task'ın 00:01:00 saatindeki çalışmasında son 1 dk lık veri aldığında 50-60. saniye arası veriler gel**me**yecektir. Çünkü 50-60 saniyeye ait
ham data verisi 00:01:05. saniyede yazılacaktır. İşte task çalıştığında eksik veri almasın tam veri alsın diye offset ile biraz gecikmeli çalışması 
sağlanabilir. offset 10s ayarlanırsa tast yine 01:00, 02:00 03:00. dakika aralığındaki verileri alacaktır ama çalışma saatleri 01:10, 02:10, 03:10 olacaktır.
Böylelikle ham data da da 50-60. saniyeler arası veri bulunmuş olacaktır.
