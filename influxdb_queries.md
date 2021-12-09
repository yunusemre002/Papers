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

```
influx delete \
  --bucket "boyahane_VTAG" \
  --start 2021-01-01T00:00:00Z \
  --stop 2021-12-01T00:00:00Z \
  --org "eliar" \
  --predicate '_measurement="machine_info"'
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
* istenilen aralık range ile getirtilir. `consumption` bilgisi sürekli artan olarak kaydedildiği
için range ile bu aralık getirtilir.
* `aggregateWindow()` kullanılarak, olası "saniyede 1 kayıt olmaması" durumunun önüne geçilmiş olunur.
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
  
 Alternatif çözüm: ilgili range için fist and last row alınabilirdi ama mesela 10 saatlik bir aralık için sadece ortada 2 saatlik bir veri varsa
 bu verinin başlangıç bitiş değerlerini alır ve 10 saatlik bir işlemin tüketim bilgisini 2 saatlik tüketim verisi ile doldurur. Yanlış hesaplamalara sebebiyet verir.
 
`hassasiyet_derecesi` isimli extra bir column eklenebilir. Bu column, aggregateWindow ile doğru orantılı olur. 
aggregateWindow = 2s ise hassasiyet derecesi de 2s olur. Eğer aggregateWindow=2s iken start || end bilgisi 
bulunamazsa, bulunamayan için hassasiyet derecesi arttırılıp birdaha sorgu yapılır. Derece tali 1m olana
kadar arttırılır. Bu arttırım sonunda hala bir consumption değeri bulunamazsa (influxdb de yoktur.) 
ilgili aralık için tüketim değeri hesaplanamaz.

