# TPC-H-MapR-DB

Running the TPC-H Benchmark on Hive
August, 10th, 2009

The official TPC-H specification can be found at: http://www.tpc.org/tpch/spec/tpch2.8.0.pdf.
"DBGEN" which generates the TPC-H test data set can be found at: http://www.tpc.org/tpch/spec/tpch_2_8_0.zip

Questions about this benchmark? Please email to Yuntao Jia at yjia@facebook.com or
comment on the Hive JIRA at https://issues.apache.org/jira/browse/HIVE-600

-----
This README covers the following topics.
1. How to set up Hadoop and Hive
2. How to generate/prepare the data
3. How to run the queries.


----
1. How to set up Hadoop and Hive.

We used Hadoop release 0.18.3. It can be downloaded at http://hadoop.apache.org/core/releases.html.
We used Hive trunk version 799148. To download Hive, please follow the instructions at http://wiki.apache.org/hadoop/Hive/GettingStarted.

The configurations of hadoop can be found at ./confs, including hadoop-env.sh and hadoop-site.xml. To adopt those files to your own systems, the following properties need to be modified:
In hadoop-env.sh:
"JAVA_HOME"
In hadoop-site.xml:
"fs.default.name"
"mapred.job.tracker"
"dfs.data.dir"
"mapred.local.dir"
"dfs.http.address"

You also need to modify the slaves file and master file to set up your cluster. In our experiment, we set up hadoop on a 11-node-cluster. One of the machine is used as the master/NameNode/JobTracker, ten others are used as slaves. We also installed Hive and PIG on the master machine.

A group of system paths need to be exported:
export HADOOP_HOME='YOUR HADOOP HOME DIRECTORY'
export HADOOP_CONF_DIR="$HADOOP_HOME/conf"
export HIVE_HOME='YOUR HIVE HOME DIRECTORY'

Lempel-Ziv-Oberhumer (lzo) compression is used in hadoop to compress intermediate map output data. To turn it off, simply change "mapred.compress.map.output" to "false" in hadoop-site.xml. Enabling lzo compression requires installing lzo libraries On all the cluster machines. They can be downloaded at http://www.oberhumer.com/opensource/lzo/. After lzo is installed, find out the path that contains all the libraries. Make sure that path is added to "/etc/ld.so.conf" which includes all the shared library loading paths. Also make sure to run "/sbin/ldconfig" to update those paths.

----
2. How to generate/prepare the data

The data is generated using the DBGEN software on TPC-H website. In our experiment, we used the 100GB dataset. See README in the DBGEN install package on details of how to generate the dataset.

After the dataset is generated, they need to be loaded in to Hadoop distributed file system (HDFS). There is a script for doing that under ./data directory. But first you have to move all the dataset to that directory. Then you can upload them to HDFS by execute the following command:
./tpch_prepare_data.sh

After running the script, you can check the data on HDFS with the following command:
$HADOOP_HOME/bin/hadoop fs -ls /tpch

----
3. How to run the queries

Make sure you have exported "HADOOP_HOME" and "HIVE_HOME" as metioned in section 1. Then you can run those queries by running the script "tpch_benchmark.sh". There are some optional settings in benchmark.conf, such as "NUM_OF_TRIALS" which is defaulted to 6.

-----

Questions about this benchmark? Please email to Yuntao Jia at yjia@facebook.com or
comment on the Hive JIRA at https://issues.apache.org/jira/browse/HIVE-600

# TPC-H_for_maprdb

USAGE:
Create table in MapR-DB-JSON:

create /part
create /partsupp
create /supplier
create /lineitem
create /orders
create /customer
create /nation
create /region

cd TPC-H-Hive
./generate.sh 300