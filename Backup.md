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
## Backup

`pg_dump -U postgres -W -F t dvdrental > c:\pgbackup\dvdrental.tar`  
  or  
`pg_dump -U postgres -Fc digitalfactory > /home/backup_file_v3.dump`

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

  ### Backup for just schema

  `pg_dump -U postgres --schema-only > c:\pgdump\definitiononly.sql`  
  
 
> The most flexible output file formats are the “custom” format (-Fc) and the “directory” format (-Fd). They allow for selection and reordering of all archived items, support parallel restoration, and are compressed by default. The “directory” format is the only format that supports parallel dumps. 
> [Neden Costom format kullanamlıyız adlı çalışma](http://zevross.com/blog/2014/06/11/use-postgresqls-custom-format-to-efficiently-backup-and-restore-tables/)

 
 ## Restore
 Before restoring a database, you need to terminate all connections to that database and prepare the backup file. In PostgreSQL, you can restore a database in two ways: `psql` and `pg_restore`

> The  `pg_restore` allows you to perform parallel restores using the  -j option to specify the number of threads for restoration. Each thread restores a separate table simultaneously, which speeds up the process dramatically. Currently, the  pg_restore support this option for the **only custom file** format.  
psql  

```
psql -U digitalfactory -c "CREATE DATABASE mynewdb"
pg_restore -U digitalfactory --dbname=mynewdb --clean /home/backup_v1.tar
```

### Restore Just schema from tar all db backup
First, create a new database named dvdrental_tpl.  
`CREATE DATABASE dvdrental_tpl;`
Second, restore the table structure only from the dvdrental.tar backup file by using the following command:
`pg_restore -U digitalfactory --dbname=dvdrental_tpl --create --section=pre-data  c:\pgbackup\dvdrental.tar`

  - **-c**  or **--clean**
    - Clean (drop) database objects before recreating them. (Unless --if-exists is used, this might generate some harmless error messages, if any objects were not    present in the destination database.)  

  - **-C** or **--create**  
    - Create the database before restoring into it. If --clean is also specified, drop and recreate the target database before connecting to it.

  - **-d** *dbname*  or **--dbname=*dbname***  
    - Connect to database dbname and restore directly into the database. The dbname can be a connection string. If so, connection string parameters will override any conflicting command line options. 
    
  - **-f** *filename* or **--file=filename**  
    - Specify output file for generated script, or for the listing when used with -l. Use - for stdout.
  
  - **-s** or **--schema-only**  
    - Restore only the schema (data definitions), not data, to the extent that schema entries are present in the archive.
  
  [postgresql-backup](https://www.postgresqltutorial.com/postgresql-backup-database/)
