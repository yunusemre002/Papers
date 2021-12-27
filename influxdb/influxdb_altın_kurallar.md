# 1. **Influxdb sorgu ekranına güvenme. Sonuçlar sıkıntılı ise sorun TİME RANGE'dir.**
*  Ham data da 2 farklı measurement eğer 2 farklı timestamp'e sahipse birleştirmek sıkıntı. Hatta saçma. Aynı timestamp olması lazım ki birleştirilebilsin. 
*  Eğer timeshift yapılacaksa en son yapılmalı. Aksi taktide sorguların 20 kat yavaşlar. `|> timeShift(duration: 3h, columns: ["_start", "_stop", "_time"])`
* Birtakım işlemler yapmadan önce mutlaka **sort** yap! `|> sort(columns: ["_value"], desc: false)`
