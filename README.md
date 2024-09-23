# MySQL Migration Using Flyway

## Introduction

Flyway is an open-source database migration tool that helps you automate and manage your database migrations. It is a lightweight and easy-to-use tool that works with plain SQL scripts. Flyway is available for multiple databases including MySQL, PostgreSQL, Oracle, SQL Server, and many more.

## Files

```sh
$ tree -a -L 1
.
|-- .env.sample
|-- .git
|-- .gitignore
|-- README.md
|-- docker-compose.yml
`-- my.cnf

1 directory, 6 files
```

Summary

- **.env.sample**: Template for .env (non-sensitive).
- **.gitignore**: Specifies files/directories to ignore in Git.
- **README.md**: Project overview and instructions.
- **docker-compose.yml**: Docker Compose configuration.
- **my.cnf**: MySQL server configuration.

Create your own `.env` with the values from the sample file.
