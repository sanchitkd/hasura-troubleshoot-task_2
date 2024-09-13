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
4. Run `docker exec -it hasura-troubleshoot-task_2-postgres-1 bash` to enter the Container. 
5. Run `psql -U postgres -d chinook;` to login to the DB.
6. Make sure the tables are listed by running `\dt`.
7. Access Hasura Console
  - Open `http://localhost:8111` in your browser. (If you have already accessed this URL, please open the link in an incognito window to avoid any duplicate data due to the browser caching.)
  - Enter admin-secret: `myadminsecretkey`
8. Expose the unexposed Tables or views over the GraphQL API:
  - Click on the Data tab in the top menu.
  - Under Databases navigate to default > public.
  - If there are Untracked tables or views, Click on Track All.
9. Apply the metadata.
- `hasura metadata apply --endpoint http://localhost:8111 --admin-secret "myadminsecretkey"`
- Manual Steps:
  - Click on the “Data” tab in the top menu.
  - Select the albums Table:
    - In the left sidebar, click on the albums table.
  - Go to the Permissions Tab:
    - Click on the “Permissions” tab.
  - Add the artist Role:
    - In the table with columns: Role, insert, select, update, delete, find the second row with the column labeled "Enter new role".
    - Enter the name artist in the "Enter new role" column.
  - Configure Select Permissions:
    - Click on the cross (×) under "select" for the artist role to change it to a tick (✓).
	  - This will open a modal or section where you can define row-level permissions.
  - Set Row-Level Permissions:
    - In the row-level permissions section, select "With Custom check".
    - Enter the following JSON to set the filter:
```
{
  "artist_id": {
    "_eq": "x-hasura-artist-id"
  }
}
```
  - Allow role artist to access columns: Toggle All
  - This filter ensures that artists can only access albums where the artist_id column matches the x-hasura-artist-id session variable.
  - Save the Permissions:
    - Click "Save Permissions" or the equivalent button to save the row-level permissions for the artist role.

## TASK CLEANUP:
- `docker-compose down -v`
- `for /f "tokens=*" %i in ('docker images -q') do docker rmi -f %i`
## CLEANUP CHECK:
- `docker-compose ps -a`
- `docker volume ls`
- `docker image ls -a`
- `docker network ls`
- `docker system df`
## TROUBLESHOOT:
- `docker-compose config`
- `docker inspect hasura-troubleshoot-task_2-postgres-1`
- `docker logs hasura-troubleshoot-task_2-postgres-1`
