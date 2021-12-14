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
 
 
# Convert time to T Z format
```
from datetime import datetime
format_date = datetime.utcnow().isoformat()[:-3]+'Z'
```
```
print(datetime.now())                         2021-12-14 15:53:55.746994
print(datetime.utcnow())                      2021-12-14 12:53:55.747053
print(datetime.utcnow().isoformat())          2021-12-14T12:53:55.747063
print(datetime.utcnow().isoformat() + "Z")    2021-12-14T12:53:55.747071Z
print(datetime.utcnow().isoformat()[:-3]+'Z') 2021-12-14T12:53:55.747Z
```
