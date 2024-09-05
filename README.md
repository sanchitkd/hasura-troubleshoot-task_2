Hasura GraphQL with Chinook Database

## Pre-requisites:
- Please insall git if not already installed. `winget install --id Git.Git -e --source winget`
- Please install Docker if not already installed and run `docker ps` to make sure there are containers running.

## Setup Steps
1. Clone the Repository
`git clone https://github.com/sanchitkd/hasura-troubleshoot-task_2.git`
2. Navigate to Directory
`cd hasura-troubleshoot-task_2`
3. Start Services
`docker-compose up -d`
4. Run `docker cp Chinook_PostgreSql.sql hasura-troubleshoot-task_2-postgres-1:/Chinook_PostgreSql.sql` to mount the DB on the postgres container
5. Run `docker exec -it hasura-troubleshoot-task_2-postgres-1 bash` to enter the Container. 
6. Run `psql -U postgres -d chinook -f Chinook_PostgreSql.sql` to execute the the sql script.
7. Run `psql -U chinook_user -d chinook;` to login to the DB.
8. Make sure the tables are listed by running `\dt`.
9. Apply the metadata.
- `hasura metadata apply --endpoint http://localhost:8111 --admin-secret "myadminsecretkey"`
10. Access Hasura Console
  - Open `http://localhost:8111` in your browser.
  - Provide the Admin secret: `myadminsecretkey`