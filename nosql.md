# NoSQL Databases Guide

This guide covers various NoSQL databases, including MongoDB, Redis, Cassandra, and Couchbase.

## MongoDB

MongoDB is a document-oriented NoSQL database.

### Installation

```bash
sudo apt-get install gnupg
wget -qO - https://www.mongodb.org/static/pgp/server-5.0.asc | sudo apt-key add -
echo "deb [ arch=amd64,arm64 ] https://repo.mongodb.org/apt/ubuntu focal/mongodb-org/5.0 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-5.0.list
sudo apt-get update
sudo apt-get install -y mongodb-org
```

### Basic Operations

1. Start MongoDB:
   ```bash
   sudo systemctl start mongod
   ```

2. Connect to MongoDB:
   ```bash
   mongo
   ```

3. Create a database:
   ```javascript
   use mydb
   ```

4. Insert a document:
   ```javascript
   db.users.insertOne({name: "John Doe", age: 30})
   ```

5. Query documents:
   ```javascript
   db.users.find({name: "John Doe"})
   ```

## Redis

Redis is an in-memory data structure store, used as a database, cache, and message broker.

### Installation

```bash
sudo apt-get update
sudo apt-get install redis-server
```

### Basic Operations

1. Start Redis:
   ```bash
   sudo systemctl start redis-server
   ```

2. Connect to Redis:
   ```bash
   redis-cli
   ```

3. Set a key-value pair:
   ```
   SET mykey "Hello"
   ```

4. Get a value:
   ```
   GET mykey
   ```

## Cassandra

Cassandra is a wide-column store NoSQL database.

### Installation

```bash
echo "deb https://debian.cassandra.apache.org 41x main" | sudo tee -a /etc/apt/sources.list.d/cassandra.sources.list
curl https://downloads.apache.org/cassandra/KEYS | sudo apt-key add -
sudo apt-get update
sudo apt-get install cassandra
```

### Basic Operations

1. Start Cassandra:
   ```bash
   sudo systemctl start cassandra
   ```

2. Connect to Cassandra:
   ```bash
   cqlsh
   ```

3. Create a keyspace:
   ```sql
   CREATE KEYSPACE mykeyspace WITH replication = {'class': 'SimpleStrategy', 'replication_factor': 1};
   ```

4. Create a table:
   ```sql
   USE mykeyspace;
   CREATE TABLE users (id UUID PRIMARY KEY, name text, age int);
   ```

5. Insert data:
   ```sql
   INSERT INTO users (id, name, age) VALUES (uuid(), 'John Doe', 30);
   ```

## Couchbase

Couchbase is a distributed NoSQL cloud database.

### Installation

1. Download Couchbase:
   ```bash
   wget https://packages.couchbase.com/releases/7.0.0/couchbase-server-community_7.0.0-ubuntu20.04_amd64.deb
   ```

2. Install Couchbase:
   ```bash
   sudo dpkg -i couchbase-server-community_7.0.0-ubuntu20.04_amd64.deb
   ```

### Basic Operations

1. Access the Couchbase Web Console at `http://localhost:8091`
2. Set up the cluster through the web interface
3. Create a bucket (database)
4. Use the Query Workbench to run N1QL queries

Remember, each of these databases has its own use cases, strengths, and weaknesses. Choose the one that best fits your specific needs.

## Ceph (Red Hat)

Ceph is a distributed object, block, and file storage platform.

### Installation

Ceph installation is complex and typically done on a cluster. Here's a simplified version for testing:

1. Install Ceph packages:
   ```bash
   sudo apt update
   sudo apt install ceph
   ```

2. Create a Ceph configuration file:
   ```bash
   sudo nano /etc/ceph/ceph.conf
   ```

3. Add the following content:
   ```ini
   [global]
   fsid = $(uuidgen)
   mon initial members = $(hostname)
   mon host = $(hostname)
   auth cluster required = cephx
   auth service required = cephx
   auth client required = cephx
   osd journal size = 1024
   osd pool default size = 1
   osd pool default min size = 1
   osd pool default pg num = 333
   osd pool default pgp num = 333
   osd crush chooseleaf type = 1
   ```

4. Create the Ceph monitor:
   ```bash
   sudo ceph-mon --mkfs -i $(hostname) --keyring /tmp/ceph.mon.keyring
   sudo systemctl start ceph-mon@$(hostname)
   ```

5. Create an OSD:
   ```bash
   sudo mkdir /var/lib/ceph/osd/ceph-0
   sudo ceph-osd -i 0 --mkfs
   sudo systemctl start ceph-osd@0
   ```

### Basic Operations

1. Check cluster status:
   ```bash
   sudo ceph -s
   ```

2. Create a pool:
   ```bash
   sudo ceph osd pool create mypool 128
   ```

3. Put an object in the pool:
   ```bash
   echo "Hello, Ceph" | sudo rados -p mypool put myobject -
   ```

4. Get an object from the pool:
   ```bash
   sudo rados -p mypool get myobject -
   ```

Note: This is a very basic setup and not suitable for production use. Production Ceph clusters require careful planning and typically involve multiple nodes.