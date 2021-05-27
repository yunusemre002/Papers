# **(Shift) Günlük Trend**
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

* **|> aggregateWindow(every: 1d, fn: spread, createEmpty: false, timeSrc:"_start")** denilerek 1 günlük verileri al, 
  * **spread** en büyük değerden en küçük değeri çıkart. Bu değer sürekli artan bir değer olduğu için enb -enk = gün sonundan gün başını çıkart = günlük tüketimi al. 
  * **timeSrc** ile aggWindow fonsksiyonunun default değerini yani **_stop** bilgisini **_start** bilgisi ile değiştirilmiş oldu. Böylelikle 1 günlük aggregate edilen dataya `_time` title olarak o günün tarihi yazılmış olacak. Örneklemek gerkirse. 15.05.2021-00:00:00 dan 16:05:2021-00:00:00 arasındaki bilgiyi 15:05:2021 e ait bilgidir diye kaydedecek. Bu trend gösteriminde kafa karışıklığını önlemek amaçlıdır.


# **(Shift) Toplam tüketim:**
```
import "date"
from(bucket: ${bucket_name:json})
  |> range(start: v.timeRangeStart, stop: v.timeRangeStop)
  |> timeShift(duration: 3h, columns: ["_start", "_stop", "_time"])
  |> filter(fn: (r) => r["_measurement"] == "Consumptions")
  |> filter(fn: (r) => contains( value: r["_field"] , set: ${Type:json}))
  |> filter(fn: (r) => contains(value: r["machineID"], set:${machineID:json}))
  |> filter(fn: (r) => r["shift"] == "shift-1")
  |> aggregateWindow(every: 1d, fn: spread)
  |> drop(columns: ["machineID", "department"])
  |> sum(column: "_value")
  |> yield(name: "mean")
```

* Amacımın tüm makinalara ait toplam tüketimi vermek. aggWindow öncesi zaten yukarıdaki sorgunun aynısı aggwindow sonucunda eğer birden fazla makine seçilirse bu makinelerin herbirisi için ayrı ayrı günlük toplam bilgisi oluşturulur. Biz tüm makinelerin toplamını istediğimiz için machineID ayırt edici özelliğini siliyoruz. Geriye department ayırt edici özelliği kalıyor ve veriler departmentlere göre tablelere ayrılmış oluyor. daha sonra bu department noyu siliyoruz. tüm değerlerr tek bir tabloda toplanmış oluyor son olarak sum denilip tüm value'ler toplanır.  Özetler ayrı ayrı oluşan table değerleri birleştirilip toplanmaktadır.


# **(Sum of shifts) Günlük Trend**
```
import "date"
from(bucket: "BoyahaneVTAG_Agg")
  |> range(start: 2021-05-21T21:00:00Z, stop: 2021-05-25T20:59:59Z)
  |> timeShift(duration: 3h, columns: ["_start", "_stop", "_time"])
  |> filter(fn: (r) => r["_measurement"] == "Consumptions")
  |> filter(fn: (r) => contains( value: r["_field"] , set:${Type:json}))
  |> filter(fn: (r) => contains(value: r["machineID"], set:${machineID:json}))
  |> aggregateWindow(every: 1d, fn: spread, createEmpty: false, timeSrc:"_start")
  |> group(columns: ["_time", "machineID"], mode:"by")
  |> sum(column: "_value")
  |> group(columns: ["machineID"], mode:"by")
  |> yield(name: "mean")
```
* Amaç shiftten bağımsız olarak bir günlük aggregate edilmiş verilrin trendini görmek..
* `|> group(columns: ["_time", "machineID"], mode:"by")` aynı timestampe ait birden 3 tane kayıt var bunlar shift bilgisiyle ayrılıyorlar ve ayrı tablolar oluşturuyorlar. Bu 3 ayrı bilgiyi bir şekilde tek çatı altında toplamanın bir yolu grup fonsksiyonudur. aynı timestamp ve machineID bilgisini aynı anda barındıran satırları bir tablo olarak gösterme işini yapmaktadır. Bu fonksiyon sonrası ve öncesi tablolar değişir. yeni bir tablo düzeni oluşur.  

  ```

            _start                   _stop                _field    _measurement    department   location machineID  shift  _value      _time 
  2021-05-20T16:20:02.811Z  2021-05-27T16:20:02.811Z  CONS_HOT_WATER  Consumptions  department-2  ÖZEN       20     shift-1  1280   2021-05-27T00:00:00.000Z  **

            _start                   _stop                _field      _measurement   department  location machineID  shift  _value      _time 
  2021-05-20T16:20:02.811Z  2021-05-27T16:20:02.811Z  CONS_HOT_WATER  Consumptions  department-2  ÖZEN       20     shift-2  1014   2021-05-26T00:00:00.000Z
  2021-05-20T16:20:02.811Z  2021-05-27T16:20:02.811Z  CONS_HOT_WATER  Consumptions  department-2  ÖZEN       20     shift-2  1290   2021-05-27T00:00:00.000Z  **

            _start                   _stop                _field      _measurement   department  location machineID  shift  _value      _time  
  2021-05-20T16:20:02.811Z  2021-05-27T16:20:02.811Z  CONS_HOT_WATER  Consumptions  department-2   ÖZEN      20     shift-3   0     2021-05-25T00:00:00.000Z
  2021-05-20T16:20:02.811Z  2021-05-27T16:20:02.811Z  CONS_HOT_WATER  Consumptions  department-2   ÖZEN      20     shift-3  1253   2021-05-26T00:00:00.000Z
  2021-05-20T16:20:02.811Z  2021-05-27T16:20:02.811Z  CONS_HOT_WATER  Consumptions  department-2   ÖZEN      20     shift-3  2653   2021-05-27T00:00:00.000Z  **
  ```
  yukarıdaki tablo group oladan ki sorgunun bir kısmını içermektedir. görüldüğü gibi aynı timestampli 3 ayrı table'da bilgiler vardır bu bilgileri tek bir tabloda toplamak için ilgili komut uygulanır ve 27. günün verileri aşağıdaki gibi 1 tablede birleştirilmiş olur.

  ```
           _start                   _stop                 _field      _measurement  department   location machineID  shift  _value      _time  
  2021-05-20T16:57:04.405Z  2021-05-27T16:57:04.405Z  CONS_HOT_WATER  Consumptions  department-5    ÖZEN    20       shift-1  1280   2021-05-27T00:00:00.000Z
  2021-05-20T16:57:04.405Z  2021-05-27T16:57:04.405Z  CONS_HOT_WATER  Consumptions  department-5    ÖZEN    20       shift-2  1290   2021-05-27T00:00:00.000Z
  2021-05-20T16:57:04.405Z  2021-05-27T16:57:04.405Z  CONS_HOT_WATER  Consumptions  department-5    ÖZEN    20       shift-3  2763   2021-05-27T00:00:00.000Z
  ```
  Daha sonra bu bilgi sum edilerek toplanır.
  sonra tekrar groupby machineID denilerek aynı makinerler ait bilgilerin tek bir tabloda gösterilmesi sağlanır.
  
