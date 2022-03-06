# Redis
database on the RAM.<br>
#### Redis on windows:
[This](https://github.com/microsoftarchive/redis/releases) link have redis on windows
```Powershell
cd \redis-dir\
# will automatic start the serivce
.\redis-server --service-install --requirepass <secret pass>
# linux
redis-server --requirepass <secret pass>
```
```Powershell
.\redis-server --service-start
# to stop the service
.\redis-server --service-stop
# To uninstall
.\redis-server --service-uninstall
```

#### Redis on linux:
```bash
wget http://download.redis.io/redis-stable.tar.gz
tar xvzf redis-stable.tar.gz
cd redis-stable
sudo make install
```
### Basic commands
```Redis
set key value
```

### Redis config
To add authN to redis you need to change the config file, or passing argument via the cli
```powershell
.\redis-server --service-install --requirepass <secret pass>

# connect the remote server
redis-cli -h 10.144.62.3 -p 30000
# to connect to cluster node
redis-cli -c -h hostname -p port -a password
# to "login"
AUTH <secret pass>
```

# Redis cluster P2P
all data is shared between nodes, LOL OMFG.
To backup data every master node creates a slave node that will back him up.<br>

Requiremnets:
1. port 6379 - the main data stream.
2. port 16379 - cluster bus, for errors and more side stuff for cluster.
need to be open and accessable 

Every cluster node needs to have a replica node.
The base model is 6 nodes 3 replicas and 3 masters
## start a cluster
https://redis.io/topics/cluster-tutorial
make a config file for each node<br>
**add a port if running on the same machine**
`port 7000` and 7001 etc...<br>
`redis.conf`
```
cluster-enabled yes
cluster-config-file nodes.conf
cluster-node-timeout 5000
appendonly yes
requirepass 12
masterauth 12
```

run the server with the config files and the password
```
redis-server ./redis.conf
```

create cluster
```
redis-cli --cluster create 127.0.0.1:7000 127.0.0.1:7001 127.0.0.1:7002 127.0.0.1:7003 127.0.0.1:7004 127.0.0.1:7005 --cluster-replicas 1 -a 12
```

To refresh redis database 0
```
redis-cli -h 127.0.0.1 -p 7000
flushall
cluster reset
exit
```





