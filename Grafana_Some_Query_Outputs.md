```
import "date"
from(bucket: ${bucket_name:json})
  |> range(start: v.timeRangeStart, stop: v.timeRangeStop)
  |> timeShift(duration: 3h, columns: ["_start", "_stop", "_time"])
  |> filter(fn: (r) => r["_measurement"] == "Consumptions")
  |> filter(fn: (r) => contains( value: r["_field"] , set: ${Type:json}))
  |> filter(fn: (r) => contains(value: r["machineID"], set:${machineID:json}))
  |> filter(fn: (r) => r["shift"] == "shift-1")
  |> aggregateWindow(every: 1d, fn: spread, createEmpty: false, timeSrc:"_start")
  |> yield(name: "mean")
```  

* **timeShift:** Şu akıldan çıkarılmaması gerekir. influxdb veri tabanına istediğimiz saatte verileri yazdıktan sonra o veriler **_time** etiketiyle beraber saklanır.
bu _time etiketi sorgular kısmında ileri geri oynatılark gösterilebilir. Bu gösterim ana veriyi bozmaz. Sadece o anda o sorguda ileri/geri olarak görünmek istediği için öyle gösterilmiş olur.
`|> timeShift(duration: 3h, columns: ["_start", "_stop", "_time"])` denilerek mevcut saat gösterimlerinin tümüne _start, _stop ve _time sütunlarına +3 saat ekle demektir.

* **|> aggregateWindow(every: 1d, fn: spread, createEmpty: false, timeSrc:"_start")** denilerek 1 günlük verileri al, **spread** en büyük değerden en küçük değeri çıkart. 
Bu değer sürekli artan bir değer olduğu için enb -enk = gün sonundan gün başını çıkart = günlük tüketimi al. **timeSrc** ile aggWindow fonsksiyonunun default değerini
  
