# Redis
database on the RAM.<br>
#### Redis on windows:
[This](https://github.com/microsoftarchive/redis/releases) link have redis on windows
```Powershell
cd \redis-dir\
# will automatic start the serivce
.\redis-server --service-install --requirepass <secret pass>
```
```Powershell
# to install
..\redis-server.exe --service-install 
# to start 
..\redis-server.exe --service-start
# to stop 
.\redis-server --service-stop
# To uninstall
.\redis-server --service-uninstall
```
## Cluster on windows
Start the 6 servers
```Powershell
.\redis-server.exe --port 7000 --cluster-enabled yes `
--cluster-config-file node-7000.conf --cluster-node-timeout 2000 `
--appendonly yes --requirepass 12 --masterauth 12
```
Cluster meet them<br>
node 1 -> meets everyone.
```Powershell
.\redis-cli.exe -c -a 12 -p 7000 cluster meet 127.0.0.1 7001
```
Allocate sharding slots
```Powershell
for ($i=0; $i -le 2730; $i=$i+1 ) {.\redis-cli.exe -p 7000 -a 12 CLUSTER ADDSLOTS $i}
for ($i=2731; $i -le 5461; $i=$i+1 ) {.\redis-cli.exe -p 7001 -a 12 CLUSTER ADDSLOTS $i}
for ($i=5462; $i -le 8192; $i=$i+1 ) {.\redis-cli.exe -p 7002 -a 12 CLUSTER ADDSLOTS $i}
for ($i=8193; $i -le 10923; $i=$i+1 ) {.\redis-cli.exe -p 7003 -a 12 CLUSTER ADDSLOTS $i}
for ($i=10924; $i -le 13654; $i=$i+1 ) {.\redis-cli.exe -p 7004 -a 12 CLUSTER ADDSLOTS $i}
for ($i=13655; $i -le 16383; $i=$i+1 ) {.\redis-cli.exe -p 7005 -a 12 CLUSTER ADDSLOTS $i}
```
The Better way
```Powershell
.\redis-cli.exe -p 7000 -a 12 CLUSTER ADDSLOTSRANGE 0 2730
.\redis-cli.exe -p 7001 -a 12 CLUSTER ADDSLOTSRANGE 2731 5461 
.\redis-cli.exe -p 7002 -a 12 CLUSTER ADDSLOTSRANGE 5462 8192
.\redis-cli.exe -p 7003 -a 12 CLUSTER ADDSLOTSRANGE 8193 10923
.\redis-cli.exe -p 7004 -a 12 CLUSTER ADDSLOTSRANGE 10924 13654
.\redis-cli.exe -p 7005 -a 12 CLUSTER ADDSLOTSRANGE 13655 16383
```
Setup slave nodes
realation:<br>
node -> replicate<br>
1 -> 2<br>
2 -> 3<br>
3 -> 4<br>
4 -> 5<br>
5 -> 6<br>
6 -> 1<br>

```Powershell
.\redis-cli -c -p 7000 -a 12 cluster replicate node2id
```
# Redis on linux:
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





