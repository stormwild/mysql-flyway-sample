# MySQL Migration Using Flyway

## Introduction

Flyway is an open-source database migration tool that helps you automate and manage your database migrations. It is a lightweight and easy-to-use tool that works with plain SQL scripts. Flyway is available for multiple databases including MySQL, PostgreSQL, Oracle, SQL Server, and many more.

## Files

```sh
$ tree -a -P '.env.sample|.gitignore|*' -I '.git' -L 3
.
|-- .env
|-- .env.sample        
|-- .gitignore
|-- README.md
|-- docker-compose.yml 
|-- flyway
|   `-- conf
|       `-- flyway.conf
|-- migrations
`-- my.cnf

3 directories, 7 files 
```

Summary

- **.env.sample**: Template for .env (non-sensitive).
- **.gitignore**: Specifies files/directories to ignore in Git.
- **README.md**: Project overview and instructions.
- **docker-compose.yml**: Docker Compose configuration.
- **my.cnf**: MySQL server configuration.

Create your own `.env` with the values from the sample file.

## Run the services

```sh
$ docker compose up -d
time="2024-09-23T14:26:38+08:00" level=warning msg="Found orphan containers ([mysql-flyway-sample-flyway-run-32433b6f04b3]) for this project. If you removed or renamed this service in your compose file, you can run this command with the --remove-orphans flag to clean it up."
[+] Running 2/2
 ✔ Container payroll_db  Running                                                      0.0s 
 ✔ Container flyway      Started                                                      0.5s                                                                                                                               0.4s 
```

![](./docker-desktop.png)

Sample logs

```sh
2024-09-23 14:26:41 See release notes here: https://rd.gt/416ObMi
2024-09-23 14:26:41 Database: jdbc:mysql://db:3306/payroll (MySQL 8.0)
2024-09-23 14:26:41 Successfully validated 0 migrations (execution time 00:00.026s)
2024-09-23 14:26:41 WARNING: No migrations found. Are your locations set up correctly?
2024-09-23 14:26:41 Current version of schema `payroll`: << Empty Schema >>
2024-09-23 14:26:41 Schema `payroll` is up to date. No migration necessary.
```

When the migrations folder is empty.

To prevent the container from exiting we replaced

```yml
    command: -configFiles=/flyway/conf/flyway.conf migrate
```

with

```yml
    entrypoint: ['/flyway/entrypoint.sh']
```

The `entrypoint.sh` script

```sh
#!/bin/sh
# entrypoint.sh

# Keep the container running
tail -f /dev/null
```

Keeps the container running indefinitely.

We can now call the Flyway CLI from the host machine.

```sh
# Start the containers
docker-compose up -d

# List migrations
docker exec -it flyway flyway info

# Apply migrations
docker exec -it flyway flyway migrate

# Validate migrations
docker exec -it flyway flyway validate
```

Sample output

```sh
$ docker exec -it flyway flyway info
Flyway OSS Edition 10.18.0 by Redgate

See release notes here: https://rd.gt/416ObMi
Database: jdbc:mysql://db:3306/payroll (MySQL 8.0)
Schema version: << Empty Schema >>

+----------+---------+-------------+------+--------------+-------+----------+
| Category | Version | Description | Type | Installed On | State | Undoable |
+----------+---------+-------------+------+--------------+-------+----------+
| No migrations found                                                       |
+----------+---------+-------------+------+--------------+-------+----------+


What's next:
    Try Docker Debug for seamless, persistent debugging tools in any container or image → docker debug flyway
    Learn more at https://docs.docker.com/go/debug-cli/
```
