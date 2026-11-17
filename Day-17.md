# Day 17: Install and Configure PostgreSQL

## Switch to the postgres user
`sudo -iu postgres`

## Execute the command psql
`psql`

## Create a user with password
`CREATE USER kodekloud_cap WITH PASSWORD 'LQfKeWWxWD';`

## Create a database and make the previously created user its owner
`CREATE DATABASE kodekloud_db10 OWNER kodekloud_cap;`
