# influxdb

influxdb to docker/home (run at docker C_)  
`influx backup --bucket-id <BucketID> --token <Token> <BACKUP Output Path!>`  

Docker/home to influxdb  (run at docker C_)  
`influx restore --token <Token> <Backup dosyalrının olduğu klasör>`  

One specific file can be copied TO the container like: (run at local)  
`docker cp foo.txt mycontainer:/foo.txt`  

One specific file can be copied FROM the container like: (run at local)  
`docker cp mycontainer:/foo.txt foo.txt`

-- 

`influx backup --bucket-id 4dc2f27ea414a741 --token 5bSPSf7l0ZDTlGItPL3aF7_e-4RiB8C3nwf5kw== /home/yedek`

docker/home to local (run at localde)  
`docker cp influxdb:/home/yedek C:\\influx\db_11_03`

--   
`docker cp C:\\Users\YED\Desktop\yedek influxdb:/home`  
`influx restore --token oA88Brj9Ego3Uqw3oPfC4x_KlckWR7wORvIfKSsJoxg1aFPLf0Uw== /home/yedek`  

--------------------------------------------------------------------------------------------------------------
# Postgresql

`pg_dump -U postgres -W -F t dvdrental > c:\pgbackup\dvdrental.tar`

**-U postgres:**  specifies the user to connect to the PostgreSQL database server. We used the postgres in this example.  

**-W:**  forces pg_dump to prompt for the password before connecting to the PostgreSQL database server. After you hit enter, pg_dump will prompt for the password of postgres user.  

**-F :** specifies the output file format that can be one of the following:  
  - **c:** custom-format archive file format  
  - **d:** directory-format archive  
  - **t:** tar  
  - **p:** plain-text SQL script file).  
  - In this example, we use  -F t to specify the output file as a tar file.  
   
**dvdrental:** is the name of the database that you want to back up. 

**> c:\pgbackup\dvdrental.tar** is the output backup file path.  

> `pg_dumpall` komutuda var ama bunun kullanılması önerilmiyor bunun yerine tüm db lerinizin backupını tek tek alın hem daha kısa sürer hem hangisinin ne kadar sürdüğünü görmüş olursunuz diyor.

  ### Sadece şemanın backupını almak için 

  `pg_dump -U postgres --schema-only > c:\pgdump\definitiononly.sql`  
  
  
  [postgresql-backup](https://www.postgresqltutorial.com/postgresql-backup-database/)
