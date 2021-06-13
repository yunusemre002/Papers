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

`
