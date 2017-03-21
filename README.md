# Apache Cassandra: The basics
Small Apache Cassandra recipes

This section assumes you have a single node Cassandra installation up and running. 


## NODETOOL USEFUL COMMANDS

1. Checking status

> nodetool status

2. Pulling statistics

> nodetool cfstats

3. Describing the cluster

> nodetool describecluster

4. Node info

> nodetool info

5. Many more at https://docs.datastax.com/en/cassandra/3.0/cassandra/tools/toolsNodetool.html

## LOADING SOME DEMO DATA


1. Grab a copy of https://ransomwaretracker.abuse.ch/feeds/csv/ and remove unnecessary lines

2. Create a keyspace
> CREATE KEYSPACE ransomware WITH REPLICATION  = { 'class' : 'SimpleStrategy', 'replication_factor' : '1' };

3. Use that keyspace
> use ransomware;
  
4. Create a column family
> CREATE COLUMNFAMILY data (firstseen varchar, threat varchar, malware varchar, host varchar, url varchar, status varchar, registrar varchar, ips varchar, asn varchar, country varchar , PRIMARY KEY (firstseen)) WITH COMMENT='ransomware data';

5. Now load the data
>  COPY data (firstseen, threat, malware, host, url, status, registrar, ips, asn, country) FROM 'ransomware.csv' WITH DELIMITER = ',' AND HEADER = TRUE;

6. Some checks
> describe keyspaces;

> describe tables;

> describe table data;

> select count(*) from data 

## SAMPLE CQLSH QUERIES

1. Regular SQL examples

> SELECT threat, malware FROM data WHERE country='US' ALLOW FILTERING; 

2. Expand on mode

> expand on;

> select * from data LIMIT 1;
