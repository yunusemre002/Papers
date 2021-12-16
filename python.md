```
import pandas as pd
import numpy as np
df2 = pd.DataFrame(np.array([[10, 20, 30], [40, 50, 60], [70, 80, 90]]),
                   columns=['col1', 'col2', 'col3'])
```
`print(df2)`
```
col1  col2  col3` 
0    10    20    30  
1    40    50    60  
2    70    80    90
``` 
`print(df2.loc[0])` 
```
col1    10
col2    20
col3    30
Name: 0, dtype: int64
```

`print(df2.iloc[0])`
```
col1    10
col2    20
col3    30
Name: 0, dtype: int64
```

`print(df2.loc[0]["col1"])`  
10  
`print(df2.iloc[0]["col1"])`   
10  
 

`print(df2.col1)`  
```
0    10
1    40
2    70
```  

`print(df2.col1.values)`  
 [10 40 70]  
 
 # Aware and Naive Objects
Date and time objects may be categorized as “aware” or “naive” depending on whether or not they include timezone information.
 
# Convert datetime to T Z format
```
from datetime import datetime
format_date = datetime.utcnow().isoformat()[:-3]+'Z'
```
```
print(datetime.now())                         # 2021-12-14 15:53:55.746994
print(datetime.utcnow())                      # 2021-12-14 12:53:55.747053
print(datetime.utcnow().isoformat())          # 2021-12-14T12:53:55.747063
print(datetime.utcnow().isoformat() + "Z")    # 2021-12-14T12:53:55.747071Z
print(datetime.utcnow().isoformat()[:-3]+'Z') # 2021-12-14T12:53:55.747Z
```

# substract -10m from datetime
```
from datetime import datetime, timedelta
new_datetime = (datetime.utcnow() - timedelta(minutes=10))

```
# Add tzinfo datetime obj
Sadece verilen timezone blgisini ekler. UTC dersen datetime'a +00:00 ekler. timezone bilgisi olmayan datetime objesinin hengi zaman dilimine ait olduğunu anlayamaz tabi, aynı objeye Konum turkey dersen sonuna, +03:00 ekler.
```
from datetime import datetime 
import pytz

naive = datetime.now()
aware = pytz.timezone('Turkey').localize(naive)
print(f"naive: {naive}", "\n"
      f"aware: {aware}")

naive_UTC = datetime.utcnow()
aware_UTC = pytz.timezone('UTC').localize(naive_UTC)
print(f"naive_UTC: {naive_UTC}", "\n"
      f"aware_UTC: {aware_UTC}")
```
Output:
```
naive: 2021-12-15 16:22:37.911960 
aware: 2021-12-15 16:22:37.911960+03:00
naive_UTC: 2021-12-15 13:22:37.919537 
aware_UTC: 2021-12-15 13:22:37.919537+00:00
```

# Remove tzinfo
```
withouttzinfo = datetimewithtzinfo.replace(tzinfo=None)
```
