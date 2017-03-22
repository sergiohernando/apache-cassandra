
# Apache Cassandra: The basics
This document complies the basics when operating with Apache Cassandra. 

More info: http://cassandra.apache.org/ and https://en.wikipedia.org/wiki/Apache_Cassandra

### NODETOOL USEFUL COMMANDS

Checking status

```sh
$ nodetool status
```

Pulling statistics

```sh
$ nodetool cfstats
```
Describing the cluster

```sh
$ nodetool describecluster
```

Node info

```sh
$ nodetool info
```
Many more nodetool examples can be found at https://docs.datastax.com/en/cassandra/3.0/cassandra/tools/toolsNodetool.html

### LOADING SOME DEMO DATA

1. Grab a copy of https://ransomwaretracker.abuse.ch/feeds/csv/ and remove unnecessary lines

2. Create a keyspace
```sh
cqlsh> CREATE KEYSPACE ransomware WITH REPLICATION  = { 'class' : 'SimpleStrategy', 'replication_factor' : '1' };
```

3. Use that keyspace

```sh
cqlsh> use ransomware;
```

4. Create a column family

```sh
cqlsh:ransomware> CREATE COLUMNFAMILY data (firstseen varchar, threat varchar, malware varchar, host varchar, url varchar, status varchar, registrar varchar, ips varchar, asn varchar, country varchar , PRIMARY KEY (firstseen)) WITH COMMENT='ransomware data';
```

5. Now load the data

```sh
cqlsh:ransomware> COPY data (firstseen, threat, malware, host, url, status, registrar, ips, asn, country) FROM 'ransomware.csv' WITH DELIMITER = ',' AND HEADER = TRUE;
```

6. Some checks

```sh
cqlsh:ransomware> describe keyspaces;
```
```sh
cqlsh:ransomware> describe tables;
```
```sh
cqlsh:ransomware> describe table data;
```
```sh
cqlsh:ransomware> select count(*) from data;
```

### SAMPLE CQLSH QUERIES

1. Regular SQL examples

```sh
cqlsh:ransomware>  SELECT threat, malware FROM data WHERE country='US' ALLOW FILTERING; 
```

2. Expand on mode

```sh
cqlsh:ransomware>  expand on; 
```
```sh
cqlsh:ransomware>  SELECT * from data LIMIT 1;
```
### QUERYING VIA THE PYTHON CASSANDRA DRIVER

This method assumes you have installed the Python Cassandra driver
```sh
$ pip install cassandra-driver 
```
Sample Python routine
```python
from cassandra.cluster import Cluster
cluster = Cluster(['127.0.0.1'])
session = cluster.connect("ransomware")
rows = session.execute('select country, ips, malware from data LIMIT 25;')
for data_row in rows:
    print data_row.country, data_row.ips, data_row.malware
```

