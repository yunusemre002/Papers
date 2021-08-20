# 1. **Custom**  
Dashboardın üst kısmında seçilebilir olarak gelmesini istediğiniz stringlerin el il eyazılmasıdır. Bu stringler selectable olarak listelenir. 
Daha sonra sorgu içinden `${queryname:json}` denilerek çağırılır. Göterilmesi istenilen ile olması gereken değerler farklı olabilir bu durumda `göterilecek_değer_1 : gerçek_değer, gösterilecek_değer_2 : gerçek_değer_2` şeklinde yazım gerçekleştirilir. 
```|> filter(fn: (r) => contains( value: r["_field"] , set: ${Type:json}))```  
![Grafana_Variables_Types_1_Custom](https://github.com/yunusemre002/Papers/blob/main/photos/Grafana_Variables_Types_1_Custom.png?raw=true)

# 2. **Custom (Hidden)**  
Dashboardın üst kısmında **gözükmez.** Burada belirtilen string yine sorgu içinde alınır. Durumdan duruma Kurumdan Kuruma ayda yılda bir kez
değişecek ve değişmesi halinde dashboardda birçok yeri el ile 1 saat değiştirmek gereken durumlarda hayat kurtarıcı olarak iş görürü. 
Mesela 4 Tane dashboard planlandı her bir dashboard 40 tane görsel/sorgu içeriyor. Bu sayfalar başka bir kurumda Bucket name değiştiyse, 
tüm görsellerin içine girip  tek tek bucket name değiştirmektense bucket name kısmı değşken olark tanımlanıp variable olarak verilebilir. 
Variable de hidden olarak ayarlanarak kullanıcıya gösterilkememiş olunur.selectable olarak listelenir. Daha sonra sorgu içinden `${queryname:json}` denilerek çağırılır.  
```from(bucket: ${bucket_name:json})```  
![Grafana_Variables_Types_2_Custom_Hide](https://github.com/yunusemre002/Papers/blob/main/photos/Grafana_Variables_Types_2_Custom_Hide.png?raw=true)


# 3. **Query**  
Veritabanından tag or field name leri onların isimlerini dönen özel sorgular yazılarak variable oalrak kaydedilir. Daha sonra bu tag field name ler selectable olarak kullanıcıya sunulur. 
Kullanıcı istediği tag/field name ile sorgulama yapabilir.  
```|> filter(fn: (r) => contains(value: r["machineID"], set:${machineID:json}))```  
![Grafana_Variables_Types_3_Query.png](https://github.com/yunusemre002/Papers/blob/main/photos/Grafana_Variables_Types_3_Query.png?raw=true)
![Grafana_Variables_Types_4_Query_1.png](https://github.com/yunusemre002/Papers/blob/main/photos/Grafana_Variables_Types_4_Query_1.png?raw=true)


```
from(bucket: ${bucket_name:json})
  |> range(start: v.timeRangeStart, stop: v.timeRangeStop)
  |> filter(fn: (r) => r["_measurement"] == "Consumptions")
  |> filter(fn: (r) => contains( value: r["_field"] , set: ${Type:json}))
  |> filter(fn: (r) => contains(value: r["machineID"], set:${machineID:json}))
  |> aggregateWindow(every: 1d, fn: spread, createEmpty: false)
```
