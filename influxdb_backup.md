influxdb to docker/home (run at docker C_)  
`influx backup --bucket-id <BucketID> --token <Token> <BACKUP Output Path!>`  

Docker/home to influxdb  (run at docker C_)  
`influx restore --token <Token> <Backup dosyalrının olduğu klasör>`  

One specific file can be copied TO the container like: (run at local)  
`docker cp foo.txt mycontainer:/foo.txt`  

One specific file can be copied FROM the container like: (run at local)  
`docker cp mycontainer:/foo.txt foo.txt`
________________________________________________________________________________________

`influx backup --bucket-id 4dc2f27ea414a741 --token 5bSPSf7l0ZDTWkkVQoCRReb_GsqEL-lGItPL3aF7_pSZ43KNkLmmfGv6nBpe-4RwfEToSNKiuzwiB8C3nwf5kw== /home/yedek`

docker/home to local (run at localde)  
`docker cp influxdb:/home/yedek C:\\influx\db_11_03`
______________________________________________________________________________________________________________________________________

`docker cp C:\\Users\YED\Desktop\yedek influxdb:/home`  
`influx restore --token oA88Brj9Ego3Uqw3oPfC4x_KlctGwCzC2CC76DkWR7wORvIfKSsJoxgE6ycvOZxD69d9P7LS6zqPW1aFPLf0Uw== /home/yedek`  
