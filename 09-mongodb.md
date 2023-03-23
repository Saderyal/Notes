# MongoDB

`mongod` is the daemon process that runs the MongoDB database.
We don't communicate directly with the `mongod`, but instead with a client (`mongosh`).

To start the mongod server on macOS on a M1 processor:

```bash
mongod --config /opt/homebrew/etc/mongod.conf --fork
```

To enter the mongo shell, run the command:

```bash
mongosh
```

Inside the mongo shell, you can shutdown the server with the command:

```bash
db.shutdownServer()
```

If you don't see a message like "connection 1 to 127.0.0.1:27017 closed", then try to first switch to the admin database with the command:

```bash
use admin
```

If you're still not sure, exit the mongo shell see the active mongod processes with the command:

```bash
ps -ef | grep mongod
```

To kill a process:

```bash
kill -9 <PID>
```

## All available mongod tools

When you install mongoDb in your machine, it also install a bunch of tools to interact with it.

You can find the tools by running the following command in the terminal (note that the path might be different. I installed it with homebrew):

```bash
find /opt/homebrew/bin -name "mongo*" 
```

Basically, the most important tools are:

* mongostats - get quick statistics on the running mongod process.
    Example:
    ```bash
    mongostat --username root --authenticationDatabase admin
    ```
* mongodump - Output a BSON. Create a Dump for a specific database
    Example:
    ```bash
    mongodump \
      --db test \
      --collection testCollection
    
    ls dump/applicationData/
    cat dump/applicationData/testCollection.metadata.json
    ```
* mongorestore - Input a BSON. Drop a collection and replace it with the one in the dump file (generate by the above command).
    Example:
    ```bash
    mongorestore --drop --port 30000 dump/
    ```
* mongoexport - Output a JSON.
    Example:
    ```bash
    mongoexport \
      --port 30000 \
      --db applicationData \
      --collection products \
      -o products.json
    ```
* mongoimport - Input a JSON.
    Example:
    ```bash
    mongoimport --port 30000 products.json
    ```

## The configuration file

By default, the mongod process starts with a default configuration (picked from /opt/homebrew/etc/mongod.conf in an M1 MacOS system), which has the following options:

- **port**: 27017
- **dbpath**: /data/db
- **bind_ip**: localhost
- **auth**: disabled

You can override this configurations by passing options in the `mongod` command, or simply override it with a custom yaml configuration file.
Here's an example:

```yaml
storage:
  dbPath: "/data/db"

systemLog:
  path: "/data/log/mongod.log"
  destination: "file"

replication:
  # This is the name of the replica set
  replSetName: "M103"

net:
  bindIp: "127.0.0.1, 192.168.0.10"
  ssl:
    mode: "requireSSL"
    PEMKeyFile: "/etc/ssl/ssl.pem"
    CAFile: "/etc/ssl/SSLCA.pem"

security:
  keyFile: "/data/keyfile"

processManagement:
  fork: true
```

Assuming that the above yaml file is in the path "/etc/mongod.conf", we can run a mongod process with that configuration with the following command:

```bash
mongod --config "/etc/mongod.conf"
```

## Mongod options

You can run the `mongod` process with some options:

- `--dbpath <directory path>` (the path of the database)
- `--logpath <directory path>` (the path of the logs)
- `--port <port number>` (the port which will expose the server)
- `--auth` (enable authentication)
- `--bind_ip <ip addresses>`
- `--fork` (tell the mongod to run as a daemon, instead of a terminal window)
- `--replSet <replica name>` (sets a replica set)
- `--keyFile <path to keyfile>` (authenticate replica sets with a keyfile)
- `--sslPEMKey <path to sslPEM key>`
- `--sslCAKey <path to sslCA key>`
- `--sslMode <mode>`
- `--config <filepath>` or `-f <filepath>` (sets a configuration file)

## File structure

If you installed mongodb via homebrew, you can see the files that the mongo process generates under the path "/opt/homebrew/var/mongodb".

> **Cauition**: You should NOT modify any of the files contained in that folder. This can cause crashes, anomalies, and loss of data.

You can see all files with the command:

```
ls -1 /opt/homebrew/var/mongodb
```

The files with the extension `.wt` are files that contains data about collections and documents.

The `WiredTiger` files are data that keeps track of informations in case the system crashes or there is a loss of power, so that the server can resume the normal execution from a specific point. Mongo create a checkpoint of data every 60 seconds, with also a file rotation.

The `diagnostic.data` folder contains data captured from the FTDC module and logs files assist support in diagnostics.

When the server starts, the `mongod.lock` file must be empty. If not (for anomalies, cruhes, or other), delete it or clean it.

Anyway, you should **NEVER** interact with any of these files, or at least refer to the documentation for detailed explanations on how to interact with them.

## Basic commands

Here I'll make a preview of the basic mongodb commands that you can use in the mongo shell.

The commands are divided in three groups:

1. `db.<method>()` - Methods to interact with the database
    * `db.<collection>.<method>()` - Interact with collections
2. `rs.<method>()` - Methods to interact with a replica set
3. `sh.<method>()` - Methods to interact with sharded clusters

All the methods we'll see here are **helper methods** that you can use in the mongo shell. If you're using a driver, you'll have to run the commands with the `db.runCommand()` function.

### User management

* db.createUser()
* db.dropUser()

### Collection management

* db.renameCollection()
* db.collection.createIndex()
* db.collection.drop()

### Database management

* db.dropDatabase()
* db.createCollection()

### Database status

* db.serverStatus()
* db.getLogComponents()
* db.setLogLevel()
* db.getProfilingStatus()
* db.setProfilingLevel()


## Logging

You can see the log components of you mongodb instance by running the command:

```bash
db.getLogComponents()
```

A verbosity level of **-1** means that it inherits the verbosity of the parent.
A verbosity level of **0** means that it logs only informational messages.
A verbosity level of **1 to 5** increase the logs to include debug messages.

If you're not trying to solve an issue, you can leave the log level to **0**.

You can see the global logs with the command:

```bash
db.adminCommand({"getLog": "global"})
```

In the logs you can see the component printing a specific line by esaminating the field "c" (e.g. "c": "NETWORK" is the network component).

You can also see the severity level of the message in the "s" field, which can be one of this 5:

* **F** - Fatal
* **E** - Error
* **W** - Warning
* **I** - Informational (verbosity level 0)
* **D** - Debug (verbosity level 1-5)

To set a verbosity level for the INDEX component (for example), you can use this command:

```
db.setLogLevel(0, "index")
```

## Profiling the database

When enabled, the MongoDB profiler will store data from all operations in a given database in a new collection called system.profile.

The events captured by the profiler are:

* **CRUD**
* **Administrative operations**
* **Configuration operations**

It has 3 level:

* **0** - The profiler is off and does not collect any data. This is the default profiler level.
* **1** - The profiler collects data for operations that take longer than the value of slowms.
* **2** - The profiler collects data for all operations. It is a bit dangerous.

You can get the profiling status of the current database by running the command:

```bash
db.getProfilingStatus()
```

You can set a new profiling level with the following command (the second parameter specifies the additional configuration for the profiler):

```bash
db.setProfilingLevel(1, { slowms: 0 })
```

After that command you can run `show collections` and see that the system created a new collection called **system.profile**.

If you try to run some commands (create collections, data, and so on) you can then run this commands and see what data the profiler registered in the system.profiler collection:

```bash
db.system.profile.find().pretty()
```

# Security

Security is always a delicate topic. In MongoDB it's even worse :D

The MongoDB security is built on the process of **authentication** (who are you?) and **authorization** (What do you have access to?).

MongoDB supports 4 Client authentication mechanisms:

1. **SCRAM** - Salted Challenge Response Auhtentication Mechanism (default, basic authentication, password authentication) 
2. **X.509** - X.509 Certificate for authentication
3. **LDAP** - Enterprise Only
4. **Kerberos** - Enterprise Only

To Authenticate every cluster between each other, MongoDB supports other authentication mechanism, but we'll see them when talking about replication and sharding.

MongoDB uses Role Based Access Control.
Each user has one or more **role**, which has one or more **privileges**, which represents a group of **actions** and the **resources** those actions apply to.

You can enable **SCRAM** authentication by passing the `--auth` flag to the mongod process or by setting the authorization flag to true in the mongod.conf file.

```yaml
security:
  authorization: true
```

Even though the authentication will be enabled, the mongoDB server will not have an initial user to authenticate. In this case, there is the **Localhost exception**:

* It allows to access a MongoDB server that enforces authentication but does not yet have configured user for you to authenticate with
* Must run Mongo shell from the same host running the MongoDB server
* The localhost exception closes after you create your first user
* Always create a user with administrative privileges first.

So:

1. Connect via localhost
    ```bash
    mongosh --host 127.0.0.1:27017
    ```
2. Switch to the admin database
    ```bash
    use admin
    ```
3. Create a user with the `root` role (which is built-in).
    ```bash
    db.createUser(
      {
        user: "root",
        pwd: "root",
        roles: [ "root" ]
      }
    )
    ```
4. Now, you can exit the mongo shell and re-connect with a username and password:
    ```bash
    mongosh --host 127.0.0.1:27017 \
    --username root \
    --password root \
    --authenticationDatabase admin
    ```

> NOTE: All users should be created on the **admin** database (only for semplicity reasons). Anyway, each user can be allowed to only operate on a specific database, even if its created on the admin database.

## Roles

MongoDB has various built-in roles.

I specified built-in because you can create your custom role for specific needs of sets of users, but, we are not going to cover that topic.

The role a structurized with a set of privileges. A role can also inherit form another role and have network authentication restrictions.

Let's now analyze the built-in roles.

### Built-in Roles

For a specific Database, you have the following built-in roles:

* Database User
  * `read`
  * `readWrite`
* Database Administration
  * `dbAdmin`
  * `userAdmin`
  * `dbOwner`
* Cluster Administration
  * `clusterAdmin`
  * `clusterManager`
  * `clusterMonitor`
  * `hostManager`
* Backup/Restore
  * `backup`
  * `restore`
* Super User
  * `root`

But, there are also roles that allow a user to operate on any database:

* Database User
  * `readAnyDatabase`
  * `readWriteAnyDatabase`
* Database Administration
  * `dbAdminAnyDatabase`
  * `userAdminAnyDatabase`
* Super User
  * `root`

### Database administration

Actually, we are going to focus on **userAdmin**, **dbOwner**, and **dbAdmin**, which are for the database administration.

Let's see an example.

#### userAdmin

With the below command, we are going to create a security officer user for our database with a role of **userAdmin** in the **admin** database:

```bash
db.createUser(
  { user: "security_officer",
    pwd: "h3ll0th3r3",
    roles: [ { db: "admin", role: "userAdmin" } ]
  }
)
```

A userAdmin is the first user you should actually create, because it allows you to do any operation about users management, but it's not enable to do any data modification.

The operation a **userAdmin** can do are:

* changeCustomData
* changePassword
* createRole
* createUser
* dropRole
* dropUser
* grantRole
* revokeRole
* setAuthenticationRestriction
* viewRole
* viewUser

So, it cannot, for example, view the collections, create a database, or crete a new entry.

#### dbAdmin

Next, create a user that is allowed to actually administrate the database. This time it will has a role of **dbAdmin**:

```bash
db.createUser(
  { user: "dba",
    pwd: "c1lynd3rs",
    roles: [ { db: "test", role: "dbAdmin" } ]
  }
)
```

The newly created user can only operate on the "test" database.

The operation a **dbAdmin** can do are (only some):

* collStats
* dbHash
* dbStats
* killCursor
* listIndexes
* listCollections
* bypassDocumentValidation
* collMod
* collStats
* compact
* convertToCapped
* ...

Note that this role can only administate the database, but it cannot create or modify any new collection, data, or whatever (but it can drop the database).

#### dbOwner

Now, suppose you want to grant an existing user with a new role. You can do this with the following command:

```bash
db.grantRolesToUser('dba', [{ db: 'playground', role: 'dbOwner' }])
```

Once we do this, the `dba` user will have a `dbOwner` role in the "dba" database.

The **dbOwner** can perform any administrative action on the database: combines the privileges granted by the **readWrite**, **dbAdmin**, and **userAdmin** roles.

### User-defined roles

MongoDB provides 15 built-in roles, but they may not describe the required set of privileges requirements. Creating user-defined roles gives fine-grained control to user administrators. User admin can adhere to the **Principle of Least Privilege** (users should have the least privilege required for their intended purpose).

To create a role, you can use this command:

```bash
db.createRole(
  {
    role: "grantRevokeRolesAnyDatabase",
    privileges: [
      {
        resource: { db: "", collection: "" },
        actions: [ "grantRole", "revokeRole", "viewRole"]
      }
    ],
    roles: []
  }
)
```

The newly created "**grantRevokeRolesAnyDatabase**" will be a role that can only assign, view, and revoke roles to the users. It'll be allowed to operate on all databased and collections because the _db_ and _collection_ fields are empty.

# Replication

MongoBD uses asynchronous, statement based replication.

Replication is the concept of replication the data of a database into different databases/mongoDB instances, so that if a server crashes, another node will still be available to access.

Each **replica set** contains an exact copy of the **primary node**. If a primary node goes down, a **secondary node** replice it through "election" (a process that decides which node will be the new primary).

A replica set must be used if you want a service that uses **_Transactions_**.

The replication mechanism is based out of a protocol, which can be PV1 or PV0. The default version is the PV1, based on the **[simple Raft Protocol](http://thesecretlivesofdata.com/raft/)**.

A replica set can also be configured as an **arbiter**, which is a member that:

* Holds no data
* Can vote in an election
* Cannot become primary

Generally, alway avoid the use of an arbiter.

> You should always have an odd number of replica sets.

> Replica sets can go up to 50 members, but there can only be 7 voting members

There can also be **HIDDEN** nodes and **DELAYED** nodes.

HIDDEN nodes are nodes that are hidden form the application: they only holds data.

DELAYED nodes are nodes that contains a delayed version of the database. For example, a 1h delayed node can be used if we accidentally drop a collection: we'll have 1h to recover the data from the delayed node.

## Settin up a replica set

To set up a replication set you have to do some configuration.

First, let's assume we want to create 3 nodes: 1 primary node and 2 secondary nodes. Let's start from the primary node.
I reccomend creating a new folder where you can store the replica data and configurations.
So, first let's start by creating a configuration for the primary node:

File: node1.conf

```yaml
storage:
  dbPath: /opt/homebrew/etc/mongodb/replication/db

net:
  bindIp: 127.0.0.1, ::1
  ipv6: true
  port: 27017

security:
  authorization: enabled
  # To authorize the sets between each other
  keyFile: /opt/homebrew/etc/mongodb/replication/keyfile

# The replication configuration
replication:
  replSetName: replica

systemLog:
  destination: file
  path: /opt/homebrew/etc/mongodb/replication/mongo1.log
  logAppend: true

processManagement:
  fork: true
```

Then, let's generate a keyfile to authenticate the replication sets between each other. We can use the following command:

```bash
cd /opt/homebrew/etc/mongodb/replication/
openssl rand -base64 741 > ./keyfile
chmod 400 ./keyfile
```

The `chmod 400` command ensures that the generate file is readonly.

The configuration for the other replica sets will be very similar, so let's create two more configurations based on the first one:

```bash
cp node1.conf node2.conf
cp node1.conf node3.conf
```

Then, in the node2.conf and node3.conf files we only have to modify the log path, the port, and the dbPath.

After that, we're not done yet, unfortunately, but we're close.

We can connect to our primary node and initialize our replication. We also have to create our first user.

```bash
rs.initiate()
use admin
db.addUser({
  user: "root",
  pwd: "root",
  roles: [{ role: "root", db: "admin" }]
})
exit
```

We initialized our replication, created a root user, and exited the mongo shell. Now, we have to reconnect, but we have to specify that we are connection to our replica (called "replica"), the host name and the port. In our case, the host name is localhost (or 127.0.0.1). We also have to authenticate ourselves.

```bash
mongosh --host replica/127.0.0.1:27017 -u root -p root --authenticationDatabase admin
```

We can see see some info about the replica with the following commands:

```
# See the status of the replica
rs.status()

# See if you're in the master node
rs.isMaster()
```

We're almost done. Right now we only have our primary node in the replica, so we have to add the other two nodes (we only have to specify the host name and the port of the member):

```bash
rs.add("127.0.0.1:27018")
rs.add("127.0.0.1:27019")
```

Weeeee're done :D

If you want, you can force an election of a new primary node with the following command:

```bash
rs.stepDown()
rs.status()
```

## Configuration

Which options we have while configuring a replica set? Here's the answer. But, first, let's see the most important commands to alterate the configuration:

* `rs.add`
* `rs.initiate`
* `rs.remove`
* `rs.reconfig`
* `rs.config`

To configure a replica set, we can base our learning on the following example:

```bash
{
  _id: <string>, # The name of the replica set (must match the conf file)
  version: <int>, # Incremented when our configuration changes
  members: [ # The members of the replica set
    {
      _id: <int>, # Unique identifier of each element
      host: <string>, # hostname:port
      arbiterOnly: <boolean>, # True if it's an arbiter (default = false)
      hidden: <boolean>, # True if it's hidden (default = false)
      priority: <number>, # The priority of the node in the election (priority = 0 exclude the node in the election)
      slaveDelay: <int>, # The delay in seconds between nodes (3600 = 1h)
    }
  ]
}
```

This are only the basic settings, but there are a lot more (that we won't actually see right now).

## Getting informations

To get informations about our replica set, we can use the following commands:

* `rs.status()` - Get the status and general information of the replica
* `rs.isMaster()` - Get information about the current node
* `db.serverStatus()['repl']` - Very similar to rs.isMaster()
* `rs.printReplicationInfo()` - Only return oplog data

## The Local database

When connection to our mongodb instance, we can see that there are already 2 databases: admin, and **local**.
The local db have only one collection in a single node: **startup_log**. But, when connecting into a replica, there are a bunch more:

* me
* oplog.rs
* replset.election
* replset.minvalid
* startup_log
* system.replset
* system.rollback.id

Each collection doesn't contains much of useful information: only configuration settings or logs. The one collection that we're interested in is the **oplog.rs**, which is central to our replication mechanism.

The oplog.rs will keep track of all states being replicated in our replica set. It's a Capped collection (it's size il limited, and you can see that size with the db.oplog.rs.stats() function).

> The size of our oplog will impact the replication window, and should be closely monitored

Of course, any of the collections in the local database shouldn't be touched.

Also, keep in mind that _any operation done in the local databae it's not replicated in the other nodes_.

## Reconfiguring a replica

While it's still running, we can change our configuration. For example, adding nodes, removing nodes, or modify a node.

To add an arbiter, you can use the command:

```bash
rs.addArb("host:port")
```

To remove it:

```bash
rs.remove("host:port")
```

Then, suppose you have 4 nodes, but, like we said before, the number of voting member should be always odd. It's not necessary to remove an entire node: we only need to exclude a node from voting, and we can do that by re-applying a configuration. We choose to modify the 4th member, by giving it 0 votes and make it hidden.

```bash
var replConfiguration = rs.conf() # The old configuration
replConfiguration.members[3].votes = 0
replConfiguration.members[3].hidden = true
replConfiguration.members[3].priority = 0
rs.reconfig(replConfiguration)
```

## Read and Write operations

Read and write operations are, by default, forbidden on secondary nodes. This is to ensure consistency between data.

To enable read and write operations in a secondary node, you can use the following command on it:

```bash
rs.slaveOk()
```

If the majority of nodes goes down, a mongodb security mechanism ensure that all the remaining nodes becomes secondary nodes, so that all read and write operations are blocked.

## Upgrading database version

To replica a database updating verions scenario, suppose we have 3 nodes at v3.4 that we want to safely update at v3.6.
We start by shutting down the secondary nodes, and updating them (one at a time). Then, we can run the `db.stepDown()` command to safely trigger a new election, update the old primary node, and resume everything :D

All of this, without causing any blocking interruption for your application.

## Write concerns

There are 4 main write concern levels:

* 0 - Don't wait for acknowledgement
* 1 (default) - Wait for acknowledgement from the primary only
* \>=2 - Wait for acknowledgement from the primary and one or more secondaries
* "majority" - Wait for acknowledgement from a majority of replica set members

With any of the 4 levels, mongoDB will still replica all data between each nodes. Writing concerns are only for _you_ to keep track the durability of your inserted data.

There are a few write concern options:

* wtimeout: \<int> - the time to wait for the request write concern before marking the operation as failed
* j: \<true|false> - requires the node to commit the write operation before to the journal before returning an acknowledgement. The "majority" level automatically sets this option.

We can set a specific write concern level with the following commands:

* insert
* update
* delete
* findAndModify

For example:

```bash
db.products.insert(
  {...},
  {
    writeConcern: { w: 2 }
  }
)
```

> The trade off of a higher write concern level is the speed at which writes are commited.

# Sharding

**Sharding** is the process of horizontally scaling our database. We devide the dataset into pieces, and distribute this pieces in many servers.

The `mongos` is the router that handle where to look for a specific data.

When to shard?

One of the first things to consider when sharding is knowing if we really need that. If we can economically scale up (vertically), just do it. It will be more performant than actually sharding the whole database. Consider also the operational impact on scalability (backup time, rstore, intial sync).

If you see some of your aggregation become slower and slower, consider sharding.

We can add any number of shards, so we have to setup a router called `mongos` (pronounced "mongo ess"). How does it figure out where to redirect a request? It uses the metadata in a collection stored in a **config server**.

Each sharding server has a **primary shard**.

> **NOTE**: Sharding is a permanent operation

## Setting up a sharded cluster

To setup a sharded cluster, we first have to create a configuration server. At the end of the day, it's just a simple mongod process, with one main different: the **clusterRole**.

In the example, we're using 3 configuration servers.

```yaml
sharding:
  clusterRole: configsvr
replication:
  replSetName: sharding-csrs
security:
  keyFile: /opt/homebrew/etc/mongodb/sharding/sharding-keyfile
net:
  bindIp: localhost,127.0.0.1
  port: 26001
systemLog:
  destination: file
  path: /opt/homebrew/etc/mongodb/sharding/csrs1.log
  logAppend: true
processManagement:
  fork: true
storage:
  dbPath: /opt/homebrew/etc/mongodb/sharding/db
```

Create the same configuration file for the other 2 servers, by only replacing the port, the systemlog, and the storage to the appropriate location and value.

Start the 3 mongo servers and initiate a replica set with the `rs.initiate()` command. Then, create a super user, authenticate, and add the other node to the replica set with the `rs.add()` command.

Then, we can createour mongos configuration file:

```yaml
sharding:
  configDB: sharding-csrs/127.0.0.1:26001,127.0.0.1:26002,127.0.0.1:26003
security:
  keyFile: /var/mongodb/pki/sharding-keyfile
net:
  bindIp: localhost,127.0.0.1
  port: 26000
systemLog:
  destination: file
  path: /var/mongodb/db/mongos.log
  logAppend: true
processManagement:
  fork: true
```

Start the mongos process and connect to it with the following commands:

```bash
mongos -f mongos.conf
mongo --port 26000 -u root -p root --authenticationDatabase admin
```

You can check the status of your sharding with the command:

```bash
sh.status()
```

Finally, we have to edit our node configuration files (3 nodes) by adding a sharding configuration and specifying as a clusterRole `shardsvr`, like so:

```yaml
sharding:
  clusterRole: shardsvr
replication:
  replSetName: replica-name
```

Then, we can connect to our node and add a new sharding node with this command:

```bash
sh.addShard("replica-name/127.0.0.1:27012")
```

## The Config DB

Generally you won't need to change anything in the config database, but is contains some useful information.

The config database is created for every mongodb instance, and contains some collections that are important:

* _databases_
* _collections_
* _chunks_
* _mongos_

> Never write to this database unless necessary

## Shard keys

MongoDB uses the shard keys to divide a collection into logical groups, called **chunks**. So, the **shard keys** are used to define the boundaries the groups.

> Shard keys must exist in every document in the collection

If you specify the shard key in a query, the router can directly redirect the request into a specific shard.

> Shard key fields **must be indexed**, are **immutable** (cannot change its value) and **permanent** (you cannot unshard a collection).

To shard, you use the following commands:

```bash
sh.enableSharding("<database>") # Enable sharding
db.collection.createIndex("<field>") # Create index for your shard key
sh.shardCollection("<database>.<collection>", { shard key }) # Shard the collection
```

> Sinche v4.2 of MongoDB, shard key values are **mutable**

The goal is a shard key whose values provides good write distribution:

* Cardinality - High cardinality = many possible unique shard key values = many chunks
* Frequency - Low Frequency = low repetition of a given shard key value
* Monotonic change - Avoid shard keys that change montoically (timestamps, dates, and so on)

Summarizing, _what makes a good shard key_ is:

* **High cardinality** (lots of unique possible values)
* **Low Frequency** (very little repetition of those unique values)
* **Non-Monotonically changing** (non-linear change in values)

Basically, good shard keys provide even write distribution.

So far we only talked about the default shard keys. But, there are also the **hashed shard keys**.

Hashed shard keys provide more even distribution of monotonically-changing shard keys (dates, and so on), but it also have some drawbacks:

* Queries on ranges of shard key values are more likely to be scatter-gather
* Cannot support geographically isolated read operations using zoned sharding
* Hashed index must be on a single non-array field
* Hashed indexes don't support fast sorting

To create an hashed shard key, you can follow this commands:

```bash
sh.enableSharding("<database>")
db.collection.createIndex({"<field>": "hashed"})
sh.shardCollection(
  "<database>.<collection>", {<shard key field>: "hashed"}
)
```

# Performance

When talking about performance, of course you have to consider the hardware, so, let's start from it.

## Hardware

MongoDB is a High Performance Database. To make sure MongoDB runs at its full potential, make sure to have a good hardware.

A good RAM memory is almost necessary, and it's heavily used in queries. About the CPU, MongoDB will try to use all the CPU's cores: the more there are, the better. CPU Availability impacts the performance of MongoDB, especially if using the WiredTiger storage engine.

What about the storage? You can see the IOPS (Input/output Operations Per Second) in the following table, for each hardware:

| Type | IOPS |
| ---- | ---- |
| 7200 rpm SATA | ~ 75 - 100 |
| 15000 rpm SAS | ~ 175 - 210 |
| SSD Intel X25-E (SLC) | ~ 5000 |
| SSD Intel X25-M G2 (MLC) | ~ 8000 |
| Amazon EBS | ~ 100 |
| Amazon EBS Provisioned | up to 2000 |
| Amazon EBS Provisioned IOPS (SSD) | ~ 3000 |
| FusionIO | ~ 135000 |
| Violin Memory 6000 | ~ 1000000 |

It's highly discouraged to use RAID 0, RAID 5, and RAID 6. The RAID Level suggested is the Level 10.

Consider also networking, load balancers, and firewalls, that may affect the performance of your mongodb Deployment.

## Indexes

What problem do indexes try to solve? The answer is: slow queries.

If we don't use and index in a collection, mongoDB will scan the whole collection to search for a specific document. This is called "collection scan", and it's a O(N) operation (linear running time). Indexes keep a reference to a document in a collection, so that a specific document can be accessed drastically faster. Indexes are stored in a binary tree, so that can also be searched as fast as possible. So, we go from an `O(N)` to `O(logN)`.

Of course, nothing in life is free. Indexes slow down the writing process for new documents, and also take space in the disk. So, don't use indexes for everything. Keep theme for the strictly necessary.

When creating an index, there is a general rule to ensure that the order of the fiels matches the query's fields: an index must created with the following order:

> Equality, Sort, Range

So, for example:

```
db.collection.createIndex({ name: 1, height: 1, age: 1 })
db.collection.find({
  name: 'Lorenzo',
  age: { $gt: 18 }
}).sort({ height: 1 })
```

# Solving Problems

Solving Problems is an advanced topic in MongoDB. There can be a lot of issues, and you should be able to correct them.

The problems can be:

* Slow queries
* Resnponse time degtradation
* Application throughtput pauses
* Aborting aggregation pipelines operations
* Server incurring high disk IO
* Incorrect File System Configuration
* Incorrect MongoDB Configuration
* Application Driver Integration Misconceptions
* Connectivity Issues
* Cluster Configuration Issues

## Tools

The mongoDB tools we are going to use are the following:

* mongostat - Shows incoming operations in real time
* mongotop - Shows which collections spent time writing from and reading to
* mongoreplay - Monitor, Record, and Replay network traffic
* mongo - Mongo shell (mongosh)
* profiler - Seen before on [Profiling the database](#profiling-the-database)
* compass - The MongoDB application
* mtools - For testing replica sets and clusters

You should be familiar with all of them.

Before that, let's start simply seeing the server logs.

### Server Logs

Server logs are the first place you look to find out what went wrong. These are information that the mongo server gives about itself and its environemnt.
We already talked a little about logs previously, but let's do a refresh.

The Server Log Components are:

* ACCESS
* COMMNAD
* CONTROL
* GEO
* INDEX
* NETWORK
* QUERY
* REPL
* SHARDING
* STORAGE
* JOURNAL
* WRITE

You can use these to filter down what you're looking for.

Also, each message has its own log level, which can be one of the following:

* F - Fatal
* E - Error
* W - Warning
* I - Informational
* D - Debug

You can specify the verbosity of the logs to determine the amount of information you get in the logs. For verbosity = 0, you get all except from debug.

### The Mongo Shell

From the mongo shell we can execute commands for debugging and write javascript to automate administration tasks.

Some useful db methods are:

* `db.currentOp()` - See what are the operations that the DB is running at the moment. Each operation is a document.
* `db.killOp()` - Kill an operation given its ID (you can get an Op. ID from the "currentOp" method). You can use this method when something is taking infinite time to run.
* `db.serverStatus()` - It gives us an overview about:
  * Instance Information
  * Asserts - `db.serverStatus().asserts`
  * Connections and Network
  * Locking - `db.serverStatus().locks`
  * Operations Stats
  * Security
  * Replication Stats
  * Storage Engine Stats
  * Metrics - `db.serverStatus().metrics`

### Profiler

The profiler will keep track of every read and write operation in a database. It can be very useful (for limited time) to see what is going on.

You shouldn't keep the profiler on, because it can quickly overload read and write operation (consuming space and computational resources).

```js
db.setProfilingLevel(1) //on
db.setProfilingLevel(0) //off
```

> Always turn it off on production environments

## Server tools for analysis

`mongoperf` is a performance testing tool. The initial tests are of disk subsystem performance. Sounds like Mongo Perv., but it's not that bad.

`mongoreplay` can **monitor**, **record**, and **replay** certain operations.

You can also use `mongostat`, which gives you network statistics. 

### Mtools

The Mtools package is a set of tools for mongoDB supported by the community (is not official). It's written in python.

A general list of tools that it offers are:

* `mlogfilter`
* `mloginfo`
* `mplotqueries`
* `mlogvis`
* `mgenerate`
* `mlaunch`

### MongoDB Cloud Manager

The Cloud Manager tool is a great tool that let you see overall statistics about your MongoDB Deployment, handle backups, and replica sets.

One cool feature about the Cloud Manager, is that you can set the **Alerts**, which basically consists on receiving emails/notifications whenever a specified parameter goes above/below a certain threshold.

## Slow Queries

The response time of your system should be < 200ms. Any operation that takes > 100ms in MongoDB is considered as slow by default.
You should consider:

* Read response time
* Write response time
* End-to-End response time

The reasons for slow queries can be:

- Working set exceeding RAM
- Queries taking longer as the data set grows
- Growing pool of clients
- Unbounded array growth
- Excessive number of indexes

### Working set exceeding RAM

By using `mongostat` you can see how much memory a process is using, and with that you can easily know if there is too little amount of memory.
This can happen if the RAM doesn't have enough space to store there a collection or an index when processing data.

Check if you're limiting the amount of cache size in the configuration file:

```yaml
storage:
  dbPath: /home/vagrant/rtd
  wiredTiger:
    engineConfig:
      cacheSizeGB: 0.25 # TOO LOW!
```

### Queries taking longer as data set grows

You should not have a response time that is linear on the data size. It should be a `O(logN)`, but this only happens if you have set the right indexes.

So, basically check if that query use an appropriate index.

Also, always remember that smaller documents are better documents: always return only the data that you need.

`Mongostat`, `mlogvis`, and the `profiler`, are here to help.

### Drop in Throughput

Sometimes it can happen that all of a sudden everything becomes slow. If you suspect an increase in Long-Running queries:

* Server Logs
* db.currentOp()
* Turn on the profiler if you can afford further slowdown

The possible causes are:

* Collection Scans
* Porly anchored regex
* Inefficient index usage

### Impact of Client Application Changes

Detect application changes by:

* Keeping historical monitoring data
* Watching out for spikes in connections
* Watching out for spikes in ops/s
* Spotting long lasting, non-indexed queries

### Using mtools to identify slow queries

To identify slow queries you can use the following tools from the mtools package:

* `mloginfo` - quick summary of query shapes
* `mplotqueries` - plot queries over time
* `mlogvis` - similar to mplotqueries but will produce an HTML file to open in a browser, so you dont have to install graphical programs

With **mloginfo** you can see the most expensive queries like this:

```bash
mloginfo --queries --no-progressbar mongod.log
```

With **mplotqueries**:

```bash
mplotqueries mongod.log
```

With **mlogvis**:

```bash
mlogvis --no-browser mongod.log
```

### Missing indexes

When you query without using indexes, your query will have a slow return for sure. When building an index, you have a few options:

* Build the index on the Primary from the shell
* Build the index in the background on the Primary from the shell
* Build the index with Compass
* Build the index on each memebr of the replica set
* Use Ops Manager or Cloud Manager to build the index

## Connectivty issues

Sometimes you can get connectivity issues like a timeout error or connection denied.
The possibilities can be various: Incorrect URI, Firewalls, or things out of your control (DDoS attacks, ecc).

The most "common" issue in connectivity are **timeout errors**.

### Timeout

A timeout error is raised whenever an operation takes too much time to complete. This can happen if you set a write concern limit (**wtimeout**) too "low".

### Closing connections

Sometimes can happen that a connection constantly closes. This could be becuase the host is bad configured or for greedy connections (connections that requiers a lot of space to be allocated).

You can see the log of each connection with the command `mongostat`.

By Default, the Maximum Incoming Connections that a single node can have, are **65536**.
You have to be sure that the default configuration is not overridden like this:

```yaml
net:
  port: 27000
  maxIncomingConnections: 200
```

Another thing that may cause dropping connections, is the election of a new primary node.

# End

Another chapter ended.
I don't know what to learn anymore.
Maybe some AI.