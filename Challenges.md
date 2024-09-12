# Challenge 1: “Manifest for hasura/graphql-engine:v2.48.2-pro not found: manifest unknown: manifest unknown”
```
    C:\Users\#\Desktop\hasura-troubleshoot-task_2>docker-compose up -d
    time="2024-09-05T21:29:25+05:30" level=warning msg="C:\\Users\\#\\Desktop\\hasura-troubleshoot-task_2\\docker-compose.yaml: the attribute `version` is obsolete, it will be ignored, please remove it to avoid potential confusion"
    [+] Running 3/3
     ✘ postgres Error       context canceled                                                                                                                                                                                                9.5s
     ✘ redis Error          context canceled                                                                                                                                                                                                9.5s
     ✘ graphql-engine Error manifest for hasura/graphql-engine:v2.48.2-pro not found: manifest unknown: manifest unknown                                                                                                                    9.5s
    Error response from daemon: manifest for hasura/graphql-engine:v2.48.2-pro not found: manifest unknown: manifest unknown
    
    C:\Users\#\Desktop\hasura-troubleshoot-task_2>
```
- Solution: 
```
# Updated hasura/graphql-engine version from https://hub.docker.com/r/hasura/graphql-engine/tags:    
    # from
    hasura/graphql-engine:v2.48.2-pro
    # to
    hasura/graphql-engine:v2.43.0
    
    # Removed version & redis:
    	version: "3.6"
    	
      redis:
        image: redis:latest
        restart: always
        ports:
          - "6379:6379"
    
    # Added the ADMIN_SECRET:
        environment:
          HASURA_GRAPHQL_ADMIN_SECRET: "myadminsecretkey"     
    
    # Added USER & DB
    		environment:
          POSTGRES_USER: postgres
          POSTGRES_DB: chinook 
    
    # Updated USER, DB & PASSWORD in HASURA_GRAPHQL_METADATA_DATABASE_URL & PG_DATABASE_URL
          HASURA_GRAPHQL_METADATA_DATABASE_URL: postgres://postgres:postgrespassword@postgres:5432/chinook
          PG_DATABASE_URL: postgres://postgres:postgrespassword@postgres:5432/chinook
    
    # Updated HASURA_GRAPHQL_ENABLE_CONSOLE to true       
          HASURA_GRAPHQL_ENABLE_CONSOLE: "true"
          
    # Removed HASURA_GRAPHQL_REDIS_URL & HASURA_GRAPHQL_RATE_LIMIT_REDIS_URL        
          HASURA_GRAPHQL_REDIS_URL: "redis://redis:6379"
          HASURA_GRAPHQL_RATE_LIMIT_REDIS_URL: "redis://redis:6379"      
```
    
```
    C:\Users\#\Desktop\hasura-troubleshoot-task_2>docker-compose up -d
    [+] Running 24/24
     ✔ graphql-engine Pulled                                                                                                                                                                                                              132.1s
       ✔ 762bedf4b1b7 Pull complete                                                                                                                                                                                                        82.8s
       ✔ b8f38fcea089 Pull complete                                                                                                                                                                                                        82.9s
       ✔ bd56fcbc8d60 Pull complete                                                                                                                                                                                                       114.2s
       ✔ b5c828c3a0e1 Pull complete                                                                                                                                                                                                       114.4s
       ✔ 3554791e0edc Pull complete                                                                                                                                                                                                       115.5s
       ✔ 6430aebc96e0 Pull complete                                                                                                                                                                                                       115.6s
       ✔ 9e59637e82a4 Pull complete                                                                                                                                                                                                       121.6s
       ✔ cf8bb7cf08e7 Pull complete                                                                                                                                                                                                       123.6s
     ✔ postgres Pulled                                                                                                                                                                                                                    131.3s
       ✔ a2318d6c47ec Pull complete                                                                                                                                                                                                        11.4s
       ✔ 293ff58ffc95 Pull complete                                                                                                                                                                                                        11.4s
       ✔ e03fd0ed521a Pull complete                                                                                                                                                                                                        11.7s
       ✔ 618b85f70713 Pull complete                                                                                                                                                                                                        11.8s
       ✔ fefee7186940 Pull complete                                                                                                                                                                                                        12.2s
       ✔ 6bd68dbb7324 Pull complete                                                                                                                                                                                                        12.3s
       ✔ 2fd2333708a2 Pull complete                                                                                                                                                                                                        12.3s
       ✔ a9193c797654 Pull complete                                                                                                                                                                                                        12.5s
       ✔ 9db671ef13b2 Pull complete                                                                                                                                                                                                       122.5s
       ✔ 229510061cf7 Pull complete                                                                                                                                                                                                       122.6s
       ✔ 5d0d6d069b83 Pull complete                                                                                                                                                                                                       122.6s
       ✔ c31960c64c87 Pull complete                                                                                                                                                                                                       122.7s
       ✔ 1c0a1dca29f6 Pull complete                                                                                                                                                                                                       122.7s
       ✔ de96e3ff4e81 Pull complete                                                                                                                                                                                                       122.8s
    [+] Running 3/3
     ✔ Network hasura-troubleshoot-task_2_default             Created                                                                                                                                                                       0.1s
     ✔ Container hasura-troubleshoot-task_2-postgres-1        Started                                                                                                                                                                       1.5s
     ✔ Container hasura-troubleshoot-task_2-graphql-engine-1  Started                                                                                                                                                                       1.5s
    
    C:\Users\#\Desktop\hasura-troubleshoot-task_2>
```

# Challenge 2: “Container is restarting, wait until the container is running”
```
    C:\Users\#\Desktop\hasura-troubleshoot-task_2>docker exec -it hasura-troubleshoot-task_2-postgres-1 bash
    Error response from daemon: Container 97ea3bbf1c98d690f127eec7555d80cac6508e576c0aea892253f36231d1c38f is restarting, wait until the container is running
    
    C:\Users\#\Desktop\hasura-troubleshoot-task_2>docker ps
    CONTAINER ID   IMAGE                           COMMAND                   CREATED         STATUS                           PORTS                                   NAMES
    97ea3bbf1c98   postgres:15                     "docker-entrypoint.s…"    4 minutes ago   **Restarting (1) 33 seconds ago**                                            hasura-troubleshoot-task_2-postgres-1
    f6b7ba3e5b77   hasura/graphql-engine:v2.43.0   "/bin/sh -c '\"${HGE_…"   4 minutes ago   Up 1 second (health: starting)   0.0.0.0:8111->80/tcp, :::8111->80/tcp   hasura-troubleshoot-task_2-graphql-engine-1
```
- Logs:
```
    C:\Users\#\Desktop\hasura-troubleshoot-task_2>docker logs hasura-troubleshoot-task_2-postgres-1
    
    PostgreSQL Database directory appears to contain a database; Skipping initialization
    
    2024-09-05 16:51:04.534 UTC [1] FATAL:  database files are incompatible with server
    2024-09-05 16:51:04.534 UTC [1] DETAIL:  The data directory was initialized by PostgreSQL version 13, which is not compatible with this version 15.8 (Debian 15.8-1.pgdg120+1).
    
    PostgreSQL Database directory appears to contain a database; Skipping initialization
    
    2024-09-05 16:51:05.216 UTC [1] FATAL:  database files are incompatible with server
    2024-09-05 16:51:05.216 UTC [1] DETAIL:  The data directory was initialized by PostgreSQL version 13, which is not compatible with this version 15.8 (Debian 15.8-1.pgdg120+1).
    
    PostgreSQL Database directory appears to contain a database; Skipping initialization
    
    2024-09-05 16:51:05.883 UTC [1] FATAL:  database files are incompatible with server
    2024-09-05 16:51:05.883 UTC [1] DETAIL:  The data directory was initialized by PostgreSQL version 13, which is not compatible with this version 15.8 (Debian 15.8-1.pgdg120+1).
    
    PostgreSQL Database directory appears to contain a database; Skipping initialization
    
    2024-09-05 16:51:06.637 UTC [1] FATAL:  database files are incompatible with server
    2024-09-05 16:51:06.637 UTC [1] DETAIL:  The data directory was initialized by PostgreSQL version 13, which is not compatible with this version 15.8 (Debian 15.8-1.pgdg120+1).
    
    PostgreSQL Database directory appears to contain a database; Skipping initialization
    
    2024-09-05 16:51:07.745 UTC [1] FATAL:  database files are incompatible with server
    2024-09-05 16:51:07.745 UTC [1] DETAIL:  The data directory was initialized by PostgreSQL version 13, which is not compatible with this version 15.8 (Debian 15.8-1.pgdg120+1).
    
    PostgreSQL Database directory appears to contain a database; Skipping initialization
    
    2024-09-05 16:51:09.645 UTC [1] FATAL:  database files are incompatible with server
    2024-09-05 16:51:09.645 UTC [1] DETAIL:  The data directory was initialized by PostgreSQL version 13, which is not compatible with this version 15.8 (Debian 15.8-1.pgdg120+1).
    
    PostgreSQL Database directory appears to contain a database; Skipping initialization
    
    2024-09-05 16:51:13.184 UTC [1] FATAL:  database files are incompatible with server
    2024-09-05 16:51:13.184 UTC [1] DETAIL:  The data directory was initialized by PostgreSQL version 13, which is not compatible with this version 15.8 (Debian 15.8-1.pgdg120+1).
    
    C:\Users\#\Desktop\hasura-troubleshoot-task_2>
```
- Solution: Update postgres to 13    
```
    
      postgres:
        image: postgres:15
    ```
    
    ```
    C:\Users\#\Desktop\hasura-troubleshoot-task_2>docker-compose up -d
    [+] Running 15/15
     ✔ postgres Pulled                                                                                                                                                                                                                     31.1s
       ✔ a2318d6c47ec Already exists                                                                                                                                                                                                        0.0s
       ✔ f29ecd7d1618 Pull complete                                                                                                                                                                                                         2.4s
       ✔ 60c9fd6649d4 Pull complete                                                                                                                                                                                                         3.2s
       ✔ cab8740fdd56 Pull complete                                                                                                                                                                                                         3.3s
       ✔ 84ea3c162c46 Pull complete                                                                                                                                                                                                         7.8s
       ✔ 9633ee13b142 Pull complete                                                                                                                                                                                                         7.9s
       ✔ 0b395dd80892 Pull complete                                                                                                                                                                                                         7.9s
       ✔ 7d6567b03997 Pull complete                                                                                                                                                                                                         7.9s
       ✔ da4990b85c70 Pull complete                                                                                                                                                                                                        22.0s
       ✔ 85d5c9c180d3 Pull complete                                                                                                                                                                                                        22.1s
       ✔ 4aa58d1adbd9 Pull complete                                                                                                                                                                                                        22.1s
       ✔ 4129bfcc48c3 Pull complete                                                                                                                                                                                                        22.2s
       ✔ 21f4381d94e7 Pull complete                                                                                                                                                                                                        22.2s
       ✔ e833a193ea76 Pull complete                                                                                                                                                                                                        22.3s
    [+] Running 2/2
     ✔ Container hasura-troubleshoot-task_2-graphql-engine-1  Running                                                                                                                                                                       0.0s
     ✔ Container hasura-troubleshoot-task_2-postgres-1        Started                                                                                                                                                                       0.5s
    
    C:\Users\#\Desktop\hasura-troubleshoot-task_2>docker ps
    CONTAINER ID   IMAGE                           COMMAND                   CREATED          STATUS                             PORTS                                       NAMES
    4cdba23b3095   postgres:13                     "docker-entrypoint.s…"    17 seconds ago   **Up 16 seconds **                     0.0.0.0:5432->5432/tcp, :::5432->5432/tcp   hasura-troubleshoot-task_2-postgres-1
    62be0e009da0   hasura/graphql-engine:v2.43.0   "/bin/sh -c '\"${HGE_…"   3 minutes ago    Up 17 seconds (health: starting)   0.0.0.0:8111->80/tcp, :::8111->80/tcp       hasura-troubleshoot-task_2-graphql-engine-1
```
    
# Challenge 3: “FATAL:  role "chinook_user" does not exist”
    
```
    root@4cdba23b3095:/# psql -U chinook_user -d chinook -f Chinook_PostgreSql.sql
    psql: error: connection to server on socket "/var/run/postgresql/.s.PGSQL.5432" failed: FATAL:  role "chinook_user" does not exist
    root@4cdba23b3095:/#
    
    root@4cdba23b3095:/# psql -U postgres -c "\du"
                                       List of roles
     Role name |                         Attributes                         | Member of
    -----------+------------------------------------------------------------+-----------
     postgres  | Superuser, Create role, Create DB, Replication, Bypass RLS | {}
    
    root@4cdba23b3095:/# psql -U postgres -d chinook -f Chinook_PostgreSql.sql
    psql:Chinook_PostgreSql.sql:19: ERROR:  cannot drop the currently open database
    psql:Chinook_PostgreSql.sql:25: ERROR:  database "chinook" already exists
    You are now connected to database "chinook" as user "postgres".
```
- Solution: Use the correct DB Username and the DB Name.
```
    root@4cdba23b3095:/# psql -U postgres -d chinook;
    psql (13.16 (Debian 13.16-1.pgdg120+1))
    Type "help" for help.
    
    chinook=# \dt
                 List of relations
     Schema |      Name      | Type  |  Owner
    --------+----------------+-------+----------
     public | album          | table | postgres
     public | artist         | table | postgres
     public | customer       | table | postgres
     public | employee       | table | postgres
     public | genre          | table | postgres
     public | invoice        | table | postgres
     public | invoice_line   | table | postgres
     public | media_type     | table | postgres
     public | playlist       | table | postgres
     public | playlist_track | table | postgres
     public | track          | table | postgres
    (11 rows)
    
    chinook=#
```
    
# Challenge 4: “http://localhost:8111/ **This site can’t be reached**”
- Solution: Updated the ports from "8111:80" to "8111:8080"
```
      graphql-engine:
        image: hasura/graphql-engine:v2.43.0
        ports:
          - "8111:8080"
```
    
# Challenge 5: “Database missing from the console”
- Solution: Added HASURA_GRAPHQL_DATABASE_URL in the docker-compose.yaml file. This is used for connecting to the primary database where the application's data resides.    
```  
        environment:
          HASURA_GRAPHQL_DATABASE_URL: postgres://postgres:postgrespassword@postgres:5432/chinook
```
    
# Error 6: “While applying the metadata it failed with The system cannot find the path specified”    
```
    C:\Users\#\Desktop\hasura-troubleshoot-task_2>hasura metadata apply --endpoint http://localhost:8111 --admin-secret "myadminsecretkey"
    time="2024-09-06T00:34:24+05:30" level=fatal msg="error applying metadata \ncannot build sources from project: error parsing metadata \nobject: sources\nfile: databases.yaml\nerror: yaml tag resolver error:\ntag: !include\nfile: C:\\Users\\#\\Desktop\\hasura-troubleshoot-task_2\\metadata\\databases\\Neon\\tables\\tables.yaml\nerror: open C:\\Users\\#\\Desktop\\hasura-troubleshoot-task_2\\metadata\\databases\\Neon\\tables\\tables.yaml: The system cannot find the path specified."
    
    C:\Users\#\Desktop\hasura-troubleshoot-task_2>
```
- Solution: Under hasura-troubleshoot-task_2\metadata\databases checked the databases.yaml file. Accordingly, changed the folder name from PG to Neon and this error is bypassed.     
```
    
      tables: "!include Neon/tables/tables.yaml"
      functions: "!include Neon/functions/functions.yaml"      
```
    
# Challenge 7: “While applying the metadata it failed with tables[1].select_permissions[0].permission.filter\",\n ”
- Issue in hasura-troubleshoot-task_2\metadata\databases\Neon\tables\
```
public_albums.yaml
    select_permissions:
      - role: artist
        permission:
          columns:
            - title
            - artist_id
            - id
          filter:
            artist:
              _or:
                - id:
                    _eq: X-Hasura-User-Id
                - id:
                    _gt: 10
        comment: ""
```
- Solution: artist should not be under filter. It should directly contain the filter conditions.
```
    select_permissions:
      - role: artist
        permission:
          columns:
            - title
            - artist_id
            - id
          filter:
            _or:
              - id:
                  _eq: X-Hasura-User-Id
              - id:
                  _gt: 10
        comment: ""
```    
- Issue in hasura-troubleshoot-task_2\metadata\databases\Neon\tables\public_artists.yaml
```
    select_permissions:
      - role: artist
        permission:
          columns:
            - name
            - id
          filter:
            - id:
                _eq: X-Hasura-User-Id
        allow_aggregations: true
        comment: ""
```    
Solution: Remove the - around id
```
    select_permissions:
      - role: artist
        permission:
          columns:
            - name
            - id
          filter:
            id:
              _eq: X-Hasura-User-Id
        allow_aggregations: true
        comment: ""    
```    
```
    C:\Users\#\Desktop\hasura-troubleshoot-task_2>hasura metadata apply --endpoint http://localhost:8111 --admin-secret "myadminsecretkey"
    INFO Metadata applied
    
    C:\Users\#\Desktop\hasura-troubleshoot-task_2>
```
   
# Challenge 8: “Using init.sql file to automates the creation of this dataset.”
- Solution: Append the docker-compose.yml file.
```
  volumes:
    - db_data:/var/lib/postgresql/data
    - ./Chinook_PostgreSql.sql:/docker-entrypoint-initdb.d/init.sql  # Add this line
```
- Explaination: This is used to initialize a PostgreSQL database container with pre-existing data when the container starts.
`:/docker-entrypoint-initdb.d/init.sql`
- This part is the target path inside the Docker container where the Chinook_PostgreSql.sql file will be copied. Docker uses the directory /docker-entrypoint-initdb.d/ to automatically execute SQL scripts when the PostgreSQL container starts. This is because the PostgreSQL containers have a built-in feature where any SQL file placed in /docker-entrypoint-initdb.d/ is automatically run when the container is initialized.

# Challege 9: "No tables are automatically created after running the docker-compose."
```  
C:\Users\#\Desktop\hasura-troubleshoot-task_2>docker exec -it hasura-troubleshoot-task_2-postgres-1 bash
root@bcc9e84e8668:/# psql -U postgres -d chinook;
psql (13.16 (Debian 13.16-1.pgdg120+1))
Type "help" for help.

chinook=# \dt
Did not find any relations.
chinook=#
```
- Logs:
```
C:\Users\#\Desktop\hasura-troubleshoot-task_2>docker logs hasura-troubleshoot-task_2-postgres-1
The files belonging to this database system will be owned by user "postgres".
This user must also own the server process.

The database cluster will be initialized with locale "en_US.utf8".
The default database encoding has accordingly been set to "UTF8".
The default text search configuration will be set to "english".

Data page checksums are disabled.

fixing permissions on existing directory /var/lib/postgresql/data ... ok
creating subdirectories ... ok
selecting dynamic shared memory implementation ... posix
selecting default max_connections ... 100
selecting default shared_buffers ... 128MB
selecting default time zone ... Etc/UTC
creating configuration files ... ok
running bootstrap script ... ok
performing post-bootstrap initialization ... ok
syncing data to disk ... ok


Success. You can now start the database server using:

    pg_ctl -D /var/lib/postgresql/data -l logfile start

initdb: warning: enabling "trust" authentication for local connections
You can change this by editing pg_hba.conf or using the option -A, or
--auth-local and --auth-host, the next time you run initdb.
waiting for server to start....2024-09-12 18:25:00.900 UTC [48] LOG:  starting PostgreSQL 13.16 (Debian 13.16-1.pgdg120+1) on x86_64-pc-linux-gnu, compiled by gcc (Debian 12.2.0-14) 12.2.0, 64-bit
2024-09-12 18:25:00.904 UTC [48] LOG:  listening on Unix socket "/var/run/postgresql/.s.PGSQL.5432"
2024-09-12 18:25:00.920 UTC [49] LOG:  database system was shut down at 2024-09-12 18:25:00 UTC
2024-09-12 18:25:00.927 UTC [48] LOG:  database system is ready to accept connections
 done
server started
CREATE DATABASE


/usr/local/bin/docker-entrypoint.sh: running /docker-entrypoint-initdb.d/init.sql
2024-09-12 18:25:01.197 UTC [62] ERROR:  cannot drop the currently open database
2024-09-12 18:25:01.197 UTC [62] STATEMENT:  /*******************************************************************************
           Chinook Database - Version 1.4.5
           Script: Chinook_PostgreSql.sql
           Description: Creates and populates the Chinook database.
           DB Server: PostgreSql
           Author: Luis Rocha
           License: https://github.com/lerocha/chinook-database/blob/master/LICENSE.md
        ********************************************************************************/

        /*******************************************************************************
          WARNING: This file was generated by a tool and changes to this file
                   will be lost when this file is regenerated.
        ********************************************************************************/

        /*******************************************************************************
           Drop database if it exists
        ********************************************************************************/
        DROP DATABASE IF EXISTS chinook;
psql:/docker-entrypoint-initdb.d/init.sql:19: ERROR:  cannot drop the currently open database

PostgreSQL Database directory appears to contain a database; Skipping initialization

2024-09-12 18:25:02.509 UTC [1] LOG:  starting PostgreSQL 13.16 (Debian 13.16-1.pgdg120+1) on x86_64-pc-linux-gnu, compiled by gcc (Debian 12.2.0-14) 12.2.0, 64-bit
2024-09-12 18:25:02.510 UTC [1] LOG:  listening on IPv4 address "0.0.0.0", port 5432
2024-09-12 18:25:02.510 UTC [1] LOG:  listening on IPv6 address "::", port 5432
2024-09-12 18:25:02.519 UTC [1] LOG:  listening on Unix socket "/var/run/postgresql/.s.PGSQL.5432"
2024-09-12 18:25:02.528 UTC [28] LOG:  database system was interrupted; last known up at 2024-09-12 18:25:01 UTC
2024-09-12 18:25:02.759 UTC [28] LOG:  database system was not properly shut down; automatic recovery in progress
2024-09-12 18:25:02.762 UTC [29] FATAL:  the database system is starting up
2024-09-12 18:25:02.765 UTC [28] LOG:  redo starts at 0/15D3600
2024-09-12 18:25:02.765 UTC [28] LOG:  invalid record length at 0/15D36F8: wanted 24, got 0
2024-09-12 18:25:02.765 UTC [28] LOG:  redo done at 0/15D36B0
2024-09-12 18:25:02.803 UTC [1] LOG:  database system is ready to accept connections
2024-09-12 18:25:02.875 UTC [36] ERROR:  relation "hdb_catalog.hdb_version" does not exist at character 69
2024-09-12 18:25:02.875 UTC [36] STATEMENT:
                SELECT ee_client_id::text, ee_client_secret
                  FROM hdb_catalog.hdb_version


C:\Users\#\Desktop\hasura-troubleshoot-task_2>
```
- Explaination: The original setup faced multiple challenges. Initially, the Chinook_PostgreSql.sql script attempted to drop and recreate the chinook database using lines like DROP DATABASE IF EXISTS chinook; and CREATE DATABASE chinook;. However, this approach led to errors during Docker initialization because the PostgreSQL database inside the container was already running and connected to the chinook database. PostgreSQL cannot drop a database that is currently being used, leading to errors such as "cannot drop the currently open database" and "database chinook already exists". Additionally, even though the container was running, no tables were created inside the database, as confirmed by running \dt in the database shell.

- Root Cause:
The docker-entrypoint-initdb.d/ directory is used by PostgreSQL for one-time initialization scripts, which only run when the database directory is empty (i.e., when the container is created for the first time). Once the container was created, the script ran, but the database already existed, leading to errors and incomplete execution of the SQL script.
The lines to drop and recreate the database caused a conflict as PostgreSQL was already connected to the chinook database, preventing successful execution.

- Resolution:
  - Modifying the SQL script:
The lines responsible for dropping and creating the chinook database were commented out (-- DROP DATABASE IF EXISTS chinook; and -- CREATE DATABASE chinook;) to prevent the conflict of trying to drop or create an already existing database.
After commenting out these lines, the script continued from the point where it connects to the chinook database using \c chinook;, and then proceeds with the table creation and data population.
  - Remove Existing Database:
Stop the containers and remove the existing database to start fresh. This will clear any existing data and allow the script to run cleanly.
We can do this by stopping the container, deleting the volume associated with the Postgres container, and then restarting.
```
docker-compose down -v  # This will remove the volumes (including the existing database)

C:\Users\#\Desktop\hasura-troubleshoot-task_2>docker-compose up -d
[+] Running 3/0
 ✔ Container hasura-troubleshoot-task_2-redis-1           Running                                                                                                      0.0s
 ✔ Container hasura-troubleshoot-task_2-postgres-1        Running                                                                                                      0.0s
 ✔ Container hasura-troubleshoot-task_2-graphql-engine-1  Running                                                                                                      0.0s

C:\Users\#\Desktop\hasura-troubleshoot-task_2>docker exec -it hasura-troubleshoot-task_2-postgres-1 bash
root@33ece75263b9:/# psql -U postgres -d chinook;
psql (13.16 (Debian 13.16-1.pgdg120+1))
Type "help" for help.

chinook=# \dt
             List of relations
 Schema |      Name      | Type  |  Owner
--------+----------------+-------+----------
 public | album          | table | postgres
 public | artist         | table | postgres
 public | customer       | table | postgres
 public | employee       | table | postgres
 public | genre          | table | postgres
 public | invoice        | table | postgres
 public | invoice_line   | table | postgres
 public | media_type     | table | postgres
 public | playlist       | table | postgres
 public | playlist_track | table | postgres
 public | track          | table | postgres
(11 rows)

chinook=#
```
