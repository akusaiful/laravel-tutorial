## Setup DB configuration

open `/.env` file, configure DB variable untuk mysql : 
    
``` py    
    DB_CONNECTION=mysql
    DB_HOST=127.0.0.1
    DB_PORT=3306
    DB_DATABASE=laravel
    DB_USERNAME=root
    DB_PASSWORD=
```

```py
   DB_CONNECTION=pgsql
   DB_HOST=localhost
   DB_PORT=5432
   DB_DATABASE=laravel
   DB_USERNAME=postgres
   DB_PASSWORD=
```


## Test connection

    php artisan tinker
    DB::connection()->getPdo()

 return status

    => PDO {#4574
     inTransaction: false,
     attributes: {
       CASE: NATURAL,
       ERRMODE: EXCEPTION,
       AUTOCOMMIT: 1,
       PERSISTENT: false,
       DRIVER_NAME: "mysql",
       SERVER_INFO: "Uptime: 252516  Threads: 1  Questions: 50  Slow queries: 0  Opens: 75  Flush tables: 1  Open tables: 68  Queries per second avg: 0.000",
       ORACLE_NULLS: NATURAL,
       CLIENT_VERSION: "mysqlnd 7.4.30",
       SERVER_VERSION: "5.6.50",
       STATEMENT_CLASS: [
         "PDOStatement",
       ],
       EMULATE_PREPARES: 0,
       CONNECTION_STATUS: "127.0.0.1 via TCP/IP",
       DEFAULT_FETCH_MODE: BOTH,
     },
    }   

Reset config cache sekiranya perubahan dibuat pada file `.env` 

    php artisan cache:clear