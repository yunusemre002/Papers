```
from(bucket: "pc-inf")
  |> range(start: v.timeRangeStart, stop: v.timeRangeStop)
  |> filter(fn: (r) => r["_measurement"] == "win_cpu")
  |> filter(fn: (r) => r["_field"] == "Percent_Idle_Time" and r["instance"] == "1")
  |> aggregateWindow(every: v.windowPeriod, fn: mean, createEmpty: false)
  |> keep(columns: ["_time", "_measurement", "_value"])
  
//   |> map(fn: (r) => ({ r with new: r._value - r._value }))
//   |> difference(nonNegative: false, columns: ["_value"])
  |> map(fn: (r) => ({ r with new: r._value - r._value }))
  |> difference(nonNegative: false, columns: ["_value"])
  |> filter(fn: (r) => r._value > 0.8)
  |> yield(name: "mean")
  ```
___________________________________________________________________________
```
import "math"
diff = from(bucket: "pc-inf")
  |> range(start: v.timeRangeStart, stop: v.timeRangeStop)
  |> filter(fn: (r) => r["_measurement"] == "win_cpu" and r["_field"] == "Percent_Idle_Time" and r["instance"] == "1")
  |> keep(columns: ["_time", "_measurement", "_value"])
  |> map(fn: (r) => ({ r with real_value:  r._value }))
  |> difference(nonNegative: false, columns: ["_value"], keepFirst: true)
  |> filter(fn: (r) => math.abs(x: r._value) > 0.8)
  |> yield(name: "diff")
 ```
___________________________________________________________________________
```
import "math"
diff = from(bucket: "pc-inf")
  |> range(start: v.timeRangeStart, stop: v.timeRangeStop)
  |> filter(fn: (r) => r["_measurement"] == "win_cpu" and r["_field"] == "Percent_Idle_Time" and r["instance"] == "1")
  |> keep(columns: ["_time", "_measurement", "_value"])
  |> map(fn: (r) => ({ r with real_value:  r._value }))
//   |> difference(nonNegative: false, columns: ["_value"], keepFirst: true)
//   |> filter(fn: (r) => math.abs(x: r._value) > 0.8)
//   |> set(key: "fabrika", value: "1")
  |> yield(name: "diff")
  ```
# DELETE _MEASUREMENT
```
influx delete \
  --bucket "boyahane_VTAG_agg" \
  --start 2021-01-01T00:00:00Z \
  --stop $(date +"%Y-%m-%dT%H:%M:%SZ") \
  --predicate '_measurement="machine_info"'
```

# İki farklı measurement'i birleştirme:
iki farklı measurement taki bilgilerin (aynı timestamp,machine_name,machine_id ye sahip) aynı satırda göstrilmesi.
Drop denilerek farklılık yaratan \_measurement bilgisi düşürülür ve daha sonra (bu sralama önemli) fieldsAsCols
denilerek aynı timestamp ve tag'e ait bilgilerin tek kolonda buluşması sağlanır
```
import "influxdata/influxdb/schema"
from(bucket: "boyahane_VTAG")
  |> range(start: v.timeRangeStart, stop: v.timeRangeStop)
  |> filter(fn: (r) => r["_measurement"] == "consumptions" or r["_measurement"] == "machine_info")
  |> filter(fn: (r) => r["machine_id"] == "15")
  |> filter(fn: (r) =>  r["_field"] == "machine_type" or  r["_field"] == "CONS_COLD_WATER"
                     or r["_field"] == "CONS_NATURAL_GAS" or r["_field"] == "CONS_ELECTRICITY"
                     or r["_field"] == "CONS_HOT_WATER" or r["_field"] == "CONS_HARD_WATER" 
                     or r["_field"] == "CONS_STEAM" or r["_field"] == "CONS_WATER" 
            )
  |> first()
  |> drop(columns: ["_measurement", "_start", "_stop"])
  |> schema.fieldsAsCols()
```


# influxdb verilen süreler aralığında hesaplama yapma
influxdb de 1s aralıklarla elektrik tüketimlerinin kaydedildiği varsayıldığında, verilen aralığa ait tüketim miktarı istenmektedir.
Tüketim bilgisi **sürekli artan** olarak kaydedilmektedir.

```
import "influxdata/influxdb/schema"
from(bucket: "boyahane_VTAG")
  |> range(start: 2021-12-01T08:08:04.050Z, stop: 2021-12-01T09:08:31.040Z)
  |> filter(fn: (r) => r["_measurement"] == "consumptions")
  |> filter(fn: (r) => r["machine_id"] == "15")
  |> aggregateWindow(every: 2s, fn: last)
  |> filter(fn: (r) => string(v: r._time) =~ /(2021-12-01T08:08:06)/  
                    or string(v: r._time) =~ /(2021-12-01T09:07:30)/
            )
  |> schema.fieldsAsCols()
```

**Soru :** istenilen aralık için consumption değerlerini hesaplayınız.
**Çözüm :** 
1. istenilen aralık range ile getirtilir. `consumption` bilgisi sürekli artan olarak kaydedildiği
  için range ile bu aralık getirtilir.
  - `aggregateWindow()` kullanılarak, olası "saniyede 1 kayıt olmaması" durumunun önüne geçilmiş olunur.
  aynı zamanda ms bilgiside 00 olarak ayarlanmış olur. 
  * regex yardımıyla istenilen sattlere en yakın ve veri olması en muhtelen zamndilimleri seçilir.
    - start_time: bir sonraki çift sayıya yuvarlanacak.
    - end_time:, bir önceki çift sayıya yuvarlanacak.
  * Eğer bu aggregateWİndow için herhangi start ||| end bulunamazsa aggregateWindow -> 
    - every: 10s 
      - start_time: bir sonraki x0'lı sayıya yuvarlanacak.
      - end_time: bir önceki x0'lı sayıya yuvarlanacak.
    - every: 10m
      - start_time: bir sonraki 00'lı sayıya yuvarlanacak.
      - end_time: bir önceki 00'lı sayıya yuvarlanacak.
    Böylelikle 3 hassasiyet derecesi elde edilmiş. olacak.

    `hassasiyet_derecesi` isimli extra bir column eklenebilir. Bu column, aggregateWindow ile doğru orantılı olur. 
    aggregateWindow = 2s ise hassasiyet derecesi de 2s olur. Eğer aggregateWindow=2s iken start || end bilgisi 
    bulunamazsa, bulunamayan için hassasiyet derecesi arttırılıp birdaha sorgu yapılır. Derece tali 1m olana
    kadar arttırılır. Bu arttırım sonunda hala bir consumption değeri bulunamazsa (influxdb de yoktur.) 
    ilgili aralık için tüketim değeri hesaplanamaz.

2. Alternatif çözüm: ilgili range için fist and last row alınabilirdi ama mesela 10 saatlik bir aralık için sadece ortada 2 saatlik bir veri varsa
bu verinin başlangıç bitiş değerlerini alır ve 10 saatlik bir işlemin tüketim bilgisini 2 saatlik tüketim verisi ile doldurur. Yanlış hesaplamalara sebebiyet    verir.
   
3. Aşağıdaki sorgu yapılır ve bu koda döndürülür daha sonra kodda gerçek saatlerde n gelen saatler çıkartılarak istenilen saatten ne kadar sapma ile bilgi geldiği ölçülür bu sapmalamar extra bir kolon olarak ekleneir bu sapmaların toplamından bir değer elde edilir bu değer de hassasiyet derecesi kolonunu doldurmamıza yardımcı olur. hassasiyet dereces,i kolonunun değerlerinin neler olacağı daha düşünülmedi.
      ```
      import "influxdata/influxdb/schema"
      first = from(bucket: "boyahane_VTAG")
                  |> range(start: 2021-12-01T08:08:04.050Z, stop: 2021-12-01T09:08:31.040Z)
                  |> filter(fn: (r) => r["_measurement"] == "consumptions")
                  |> filter(fn: (r) => r["machine_id"] == "15")
                  |> first()
                  |> schema.fieldsAsCols()

      last  = from(bucket: "boyahane_VTAG")
                  |> range(start: 2021-12-01T08:08:04.050Z, stop: 2021-12-01T09:08:31.040Z)
                  |> filter(fn: (r) => r["_measurement"] == "consumptions")
                  |> filter(fn: (r) => r["machine_id"] == "15")
                  |> last()
                  |> schema.fieldsAsCols()


      union(tables: [first, last])
      |> drop(columns: ["_measurement", "_start", "_stop"])
      |> sort(columns: ["_time"], desc: false)
    ```
    Total hesabınıda influxdb ye yaptırmak için:
    ```
    import "influxdata/influxdb/schema"
    first = from(bucket: "boyahane_VTAG")
                |> range(start: 2021-12-07T08:08:04.050Z, stop: 2021-12-07T09:08:31.040Z)
                |> filter(fn: (r) => r["_measurement"] == "consumptions")
                |> filter(fn: (r) => r["machine_id"] == "75")
                |> first()

    last  = from(bucket: "boyahane_VTAG")
                |> range(start: 2021-12-07T08:08:04.050Z, stop: 2021-12-07T09:08:31.040Z)
                |> filter(fn: (r) => r["_measurement"] == "consumptions")
                |> filter(fn: (r) => r["machine_id"] == "75")
                |> last()

    total = union(tables: [first, last])
                |> sort(columns: ["_value"], desc: false)               // Önemli!
                |> difference(nonNegative: false, columns: ["_value"])
                |> map(fn: (r) => ({ r with _time: now() }))            // Son satırda yer alması için versik bunu. 

    // Bu alttakileri eklememin sebebi, hangi satırın ne ifade ettiği bilinmesi içindir.
    // Her ne kadar bu bilgiler belirli bir sıra ile verilsede ek bilgi satırının olması 
    // Okuyucu açısından daha anlamlıdır.
    first_row = first
                |> schema.fieldsAsCols()
                |> set(key: "row_info", value: "start")

    last_row  = last
                |> schema.fieldsAsCols()
                |> set(key: "row_info", value: "finish")

    total_row = total
                |> schema.fieldsAsCols()
                |> set(key: "row_info", value: "total")

    union(tables: [first_row, last_row, total_row])
    |> drop(columns: ["_measurement", "_start", "_stop"])
    |> sort(columns: ["_time"], desc: false)
    ```
# Değişimleri Bulmak (integer değerlerde)
Eğer elimizde saniye bazlı veri varsa ve bu veriler çok sık değişmiyorsa. BU sık değişmeme durumundan dolayı sadece değişimleri mi kaydetsek ya? diye düşünebiliriz. Bu kayıt şekline event based kayıt denilmektedir. Aşağıda saniye bazlı kaydı tutulan integer değerlerin değişimlerini nasıl tuutlacağına
dair sorgu bulunmaktadır.
```
from(bucket: "boyahane_VTAG")
  |> range(start: v.timeRangeStart, stop: v.timeRangeStop)
  |> filter(fn: (r) => r["_measurement"] == "machine_info")
  |> filter(fn: (r) => r["_field"] == "active_operator_no")
  |> map(fn: (r) => ({ r with org_value: r._value }))
  |> difference(nonNegative: false, columns: ["_value"])
  |> filter(fn: (r) => r._value != 0)
  |> map(fn: (r) => ({ r with _value: r.org_value }))
  |> drop(columns: ["org_value"])
```
**Bu sorgu aslında ne yapıyor?** `_value` kolonundaki değerleri `org_value` diye bir kolona kopyalıyor. Sonra `_value` kolonuna göre difference alıyor. Yani birbirini takip eden 2 satırın farkını alıp yeni bir satır oluşturuyor. Bu fark 0 olanlar siliniyor. Demekki aynı değer tekrar etmiş. Saha sonra `org_value` isimli kolona taşıdığımız esas değerler geri `_value` kolonuna alınıyor ve `org_value` isimli kolon siliniyor. Sonuç olarak sadece değişime ait değerlerin olduğu satırlar tututlmuş oluyor.
