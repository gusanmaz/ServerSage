# Database Management Guide

This guide covers the setup and management of popular database systems on a VPS.

## Table of Contents

1. [MySQL/MariaDB](#mysqlmariadb)
2. [PostgreSQL](#postgresql)
3. [MongoDB](#mongodb)

## MySQL/MariaDB

### Installation

```bash
sudo apt update
sudo apt install mysql-server
```

### Secure Installation

```bash
sudo mysql_secure_installation
```

### Basic Operations

1. Access MySQL:
   ```bash
   sudo mysql
   ```

2. Create a database:
   ```sql
   CREATE DATABASE mydatabase;
   ```

3. Create a user:
   ```sql
   CREATE USER 'myuser'@'localhost' IDENTIFIED BY 'password';
   ```

4. Grant privileges:
   ```sql
   GRANT ALL PRIVILEGES ON mydatabase.* TO 'myuser'@'localhost';
   FLUSH PRIVILEGES;
   ```

### Backup and Restore

1. Backup:
   ```bash
   mysqldump -u root -p mydatabase > backup.sql
   ```

2. Restore:
   ```bash
   mysql -u root -p mydatabase < backup.sql
   ```

## PostgreSQL

### Installation

```bash
sudo apt update
sudo apt install postgresql postgresql-contrib
```

### Basic Operations

1. Switch to postgres user:
   ```bash
   sudo -i -u postgres
   ```

2. Access PostgreSQL:
   ```bash
   psql
   ```

3. Create a database:
   ```sql
   CREATE DATABASE mydatabase;
   ```

4. Create a user:
   ```sql
   CREATE USER myuser WITH PASSWORD 'password';
   ```

5. Grant privileges:
   ```sql
   GRANT ALL PRIVILEGES ON DATABASE mydatabase TO myuser;
   ```

### Backup and Restore

1. Backup:
   ```bash
   pg_dump mydatabase > backup.sql
   ```

2. Restore:
   ```bash
   psql mydatabase < backup.sql
   ```

## MongoDB

### Installation

```bash
sudo apt update
sudo apt install mongodb
```

### Basic Operations

1. Start MongoDB:
   ```bash
   sudo systemctl start mongodb
   ```

2. Access MongoDB shell:
   ```bash
   mongo
   ```

3. Create a database:
   ```javascript
   use mydatabase
   ```

4. Create a collection:
   ```javascript
   db.createCollection("mycollection")
   ```

5. Insert a document:
   ```javascript
   db.mycollection.insertOne({ name: "John Doe", age: 30 })
   ```

### Backup and Restore

1. Backup:
   ```bash
   mongodump --db=mydatabase --out=/path/to/backup
   ```

2. Restore:
   ```bash
   mongorestore --db=mydatabase /path/to/backup/mydatabase
   ```

Remember to secure your databases by using strong passwords, restricting network access, and keeping the database software updated.