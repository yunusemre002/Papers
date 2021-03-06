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

# ??ki farkl?? measurement'i birle??tirme:
iki farkl?? measurement taki bilgilerin (ayn?? timestamp,machine_name,machine_id ye sahip) ayn?? sat??rda g??strilmesi.
Drop denilerek farkl??l??k yaratan \_measurement bilgisi d??????r??l??r ve daha sonra (bu sralama ??nemli) fieldsAsCols
denilerek ayn?? timestamp ve tag'e ait bilgilerin tek kolonda bulu??mas?? sa??lan??r
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


# influxdb verilen s??reler aral??????nda hesaplama yapma
influxdb de 1s aral??klarla elektrik t??ketimlerinin kaydedildi??i varsay??ld??????nda, verilen aral????a ait t??ketim miktar?? istenmektedir.
T??ketim bilgisi **s??rekli artan** olarak kaydedilmektedir.

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

**Soru :** istenilen aral??k i??in consumption de??erlerini hesaplay??n??z.
**????z??m :** 
1. istenilen aral??k range ile getirtilir. `consumption` bilgisi s??rekli artan olarak kaydedildi??i
  i??in range ile bu aral??k getirtilir.
  - `aggregateWindow()` kullan??larak, olas?? "saniyede 1 kay??t olmamas??" durumunun ??n??ne ge??ilmi?? olunur.
  ayn?? zamanda ms bilgiside 00 olarak ayarlanm???? olur. 
  * regex yard??m??yla istenilen sattlere en yak??n ve veri olmas?? en muhtelen zamndilimleri se??ilir.
    - start_time: bir sonraki ??ift say??ya yuvarlanacak.
    - end_time:, bir ??nceki ??ift say??ya yuvarlanacak.
  * E??er bu aggregateW??ndow i??in herhangi start ||| end bulunamazsa aggregateWindow -> 
    - every: 10s 
      - start_time: bir sonraki x0'l?? say??ya yuvarlanacak.
      - end_time: bir ??nceki x0'l?? say??ya yuvarlanacak.
    - every: 10m
      - start_time: bir sonraki 00'l?? say??ya yuvarlanacak.
      - end_time: bir ??nceki 00'l?? say??ya yuvarlanacak.
    B??ylelikle 3 hassasiyet derecesi elde edilmi??. olacak.

    `hassasiyet_derecesi` isimli extra bir column eklenebilir. Bu column, aggregateWindow ile do??ru orant??l?? olur. 
    aggregateWindow = 2s ise hassasiyet derecesi de 2s olur. E??er aggregateWindow=2s iken start || end bilgisi 
    bulunamazsa, bulunamayan i??in hassasiyet derecesi artt??r??l??p birdaha sorgu yap??l??r. Derece tali 1m olana
    kadar artt??r??l??r. Bu artt??r??m sonunda hala bir consumption de??eri bulunamazsa (influxdb de yoktur.) 
    ilgili aral??k i??in t??ketim de??eri hesaplanamaz.

2. Alternatif ????z??m: ilgili range i??in fist and last row al??nabilirdi ama mesela 10 saatlik bir aral??k i??in sadece ortada 2 saatlik bir veri varsa
bu verinin ba??lang???? biti?? de??erlerini al??r ve 10 saatlik bir i??lemin t??ketim bilgisini 2 saatlik t??ketim verisi ile doldurur. Yanl???? hesaplamalara sebebiyet    verir.
   
3. A??a????daki sorgu yap??l??r ve bu koda d??nd??r??l??r daha sonra kodda ger??ek saatlerde n gelen saatler ????kart??larak istenilen saatten ne kadar sapma ile bilgi geldi??i ??l????l??r bu sapmalamar extra bir kolon olarak ekleneir bu sapmalar??n toplam??ndan bir de??er elde edilir bu de??er de hassasiyet derecesi kolonunu doldurmam??za yard??mc?? olur. hassasiyet dereces,i kolonunun de??erlerinin neler olaca???? daha d??????n??lmedi.
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
    Total hesab??n??da influxdb ye yapt??rmak i??in:
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
                |> sort(columns: ["_value"], desc: false)               // ??nemli!
                |> difference(nonNegative: false, columns: ["_value"])
                |> map(fn: (r) => ({ r with _time: now() }))            // Son sat??rda yer almas?? i??in versik bunu. 

    // Bu alttakileri eklememin sebebi, hangi sat??r??n ne ifade etti??i bilinmesi i??indir.
    // Her ne kadar bu bilgiler belirli bir s??ra ile verilsede ek bilgi sat??r??n??n olmas?? 
    // Okuyucu a????s??ndan daha anlaml??d??r.
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
# De??i??imleri Bulmak (integer de??erlerde)
E??er elimizde saniye bazl?? veri varsa ve bu veriler ??ok s??k de??i??miyorsa. BU s??k de??i??meme durumundan dolay?? sadece de??i??imleri mi kaydetsek ya? diye d??????nebiliriz. Bu kay??t ??ekline event based kay??t denilmektedir. A??a????da saniye bazl?? kayd?? tutulan integer de??erlerin de??i??imlerini nas??l tuutlaca????na
dair sorgu bulunmaktad??r.
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
**Bu sorgu asl??nda ne yap??yor?** `_value` kolonundaki de??erleri `org_value` diye bir kolona kopyal??yor. Sonra `_value` kolonuna g??re difference al??yor. Yani birbirini takip eden 2 sat??r??n fark??n?? al??p yeni bir sat??r olu??turuyor. Bu fark 0 olanlar siliniyor. Demekki ayn?? de??er tekrar etmi??. Saha sonra `org_value` isimli kolona ta????d??????m??z esas de??erler geri `_value` kolonuna al??n??yor ve `org_value` isimli kolon siliniyor. Sonu?? olarak sadece de??i??ime ait de??erlerin oldu??u sat??rlar tututlmu?? oluyor.
