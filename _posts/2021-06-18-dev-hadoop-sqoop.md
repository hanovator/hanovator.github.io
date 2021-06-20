---
layout: post
title:  "sqoop 설치"
subtitle:   "sqoop 설치"
categories: dev
tags: hadoop
comments: true
---

## 목차
1. sqoop이란?
2. 설치

## 1. sqoop이란? 
- RDBMS 와 HDFS간의 데이터 전송지원 도구
- 외부 시스템의 데이터를 HDFS로 가져오면 Hive등 에코시스템에서 사용하고 다양한 파일 형태로 저장도 가능하다.
- JDBC와 호환되는 DB에서 사용가능
- 아래 사진과 같이 Map만 사용되며 최초 import시 metadata를 가져온다.
- ![](https://images.velog.io/images/hanovator/post/9285eeb2-045e-4158-ae5f-27cc2d7891c9/image.png)

## 2. 설치
#### 1) wget http://mirror.apache-kr.org/sqoop/1.4.7/sqoop-1.4.7.bin__hadoop-2.6.0.tar.gz
- /opt/sqoop-1.4.7 폴더에 다운 후 압축해제

#### 2) 이전 hive 설치시 사용했던 /opt/hive/mysql-connection jar파일을 sqoop-1.4.7/lib폴더에 이동해준다.
#### 3) 환경설정 추가.
```bash
export JAVA_HOME="/usr/lib/jvm/java-8-openjdk-amd64"
export HADOOP_HOME="/usr/local/hadoop"
export PATH="/opt/anaconda3/bin:$PATH"
export SPARK_HOME="/opt/spark-3.0.2"
export PYSPARK="/opt/anaconda3"
export PYSPARK_PYTHON="/opt/anaconda3/bin/python"
export HIVE_HOME="/opt/hive-3.1.2"
export SQOOP_HOME="/opt/sqoop-1.4.7"

export PATH=$SPARK_HOME/bin:$SPARK_HOME/sbin:$PATH:$JAVA_HOME/bin:$HADOOP_HOME/bin:$HADOOP_HOME/sbin:$HIVE_HOME/bin:$SQOOP_HOME/bin:
export PYSPARK_DRIVER_PYTHON="jupyter"
export PYSPARK_DRIVER_PYTHON_OPTS="notebook --allow-root"
```
#### 4) sqoop 환경설정 변경
```bash
# Licensed to the Apache Software Foundation (ASF) under one or more
# contributor license agreements.  See the NOTICE file distributed with
# this work for additional information regarding copyright ownership.
# The ASF licenses this file to You under the Apache License, Version 2.0
# (the "License"); you may not use this file except in compliance with
# the License.  You may obtain a copy of the License at
#
#     http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.


# included in all the hadoop scripts with source command
# should not be executable directly
# also should not be passed any arguments, since we need original $*

# Set Hadoop-specific environment variables here.

#Set path to where bin/hadoop is available
export HADOOP_COMMON_HOME=/usr/local/hadoop

#Set path to where hadoop-*-core.jar is available
export HADOOP_MAPRED_HOME=/usr/local/hadoop

#set the path to where bin/hbase is available
#export HBASE_HOME=

#Set the path to where bin/hive is available
export HIVE_HOME=/opt/hive-3.1.2

#Set the path for where zookeper config dir is
#export ZOOCFGDIR=
```
```bash
# version 확인
(base) root@aidw-001:/opt/sqoop-1.4.7/lib# sqoop-version
Warning: /opt/sqoop-1.4.7/../hbase does not exist! HBase imports will fail.
Please set $HBASE_HOME to the root of your HBase installation.
Warning: /opt/sqoop-1.4.7/../hcatalog does not exist! HCatalog jobs will fail.
Please set $HCAT_HOME to the root of your HCatalog installation.
Warning: /opt/sqoop-1.4.7/../accumulo does not exist! Accumulo imports will fail.
Please set $ACCUMULO_HOME to the root of your Accumulo installation.
Warning: /opt/sqoop-1.4.7/../zookeeper does not exist! Accumulo imports will fail.
Please set $ZOOKEEPER_HOME to the root of your Zookeeper installation.
2021-06-16 10:55:11,501 INFO sqoop.Sqoop: Running Sqoop version: 1.4.7
Sqoop 1.4.7
git commit id 2328971411f57f0cb683dfb79d19d4d19d185dd8
Compiled by maugli on Thu Dec 21 15:59:58 STD 2017
```

#### 5) sqoop과 database connection 확인
```bash
(base) root@aidw-001:/opt/sqoop-1.4.7/lib# sqoop list-databases --connect 'jdbc:mysql://aidw-001:3306/web?useSSL=false&allowPublicKeyRetrieval=true' --username user --password 1234
Warning: /opt/sqoop-1.4.7/../hbase does not exist! HBase imports will fail.
Please set $HBASE_HOME to the root of your HBase installation.
Warning: /opt/sqoop-1.4.7/../hcatalog does not exist! HCatalog jobs will fail.
Please set $HCAT_HOME to the root of your HCatalog installation.
Warning: /opt/sqoop-1.4.7/../accumulo does not exist! Accumulo imports will fail.
Please set $ACCUMULO_HOME to the root of your Accumulo installation.
Warning: /opt/sqoop-1.4.7/../zookeeper does not exist! Accumulo imports will fail.
Please set $ZOOKEEPER_HOME to the root of your Zookeeper installation.
2021-06-16 10:54:25,609 INFO sqoop.Sqoop: Running Sqoop version: 1.4.7
2021-06-16 10:54:25,665 WARN tool.BaseSqoopTool: Setting your password on the command-line is insecure. Consider using -P instead.
2021-06-16 10:54:25,730 INFO manager.MySQLManager: Preparing to use a MySQL streaming resultset.
mysql
information_schema
performance_schema
sys
web
hive
```

#### 6) mysql에 있는 web database 의 guestbook table를 hdfs로 이동
```bash
(base) root@aidw-001:/opt/sqoop-1.4.7/lib# sqoop import --connect 'jdbc:mysql://aidw-001:3306/web?useSSL=false&allowPublicKeyRetrieval=true' --username user --password 1234 --table guestbook
Warning: /opt/sqoop-1.4.7/../hbase does not exist! HBase imports will fail.
Please set $HBASE_HOME to the root of your HBase installation.
Warning: /opt/sqoop-1.4.7/../hcatalog does not exist! HCatalog jobs will fail.
Please set $HCAT_HOME to the root of your HCatalog installation.
Warning: /opt/sqoop-1.4.7/../accumulo does not exist! Accumulo imports will fail.
Please set $ACCUMULO_HOME to the root of your Accumulo installation.
Warning: /opt/sqoop-1.4.7/../zookeeper does not exist! Accumulo imports will fail.
Please set $ZOOKEEPER_HOME to the root of your Zookeeper installation.
2021-06-16 10:50:59,778 INFO sqoop.Sqoop: Running Sqoop version: 1.4.7
2021-06-16 10:50:59,835 WARN tool.BaseSqoopTool: Setting your password on the command-line is insecure. Consider using -P instead.
2021-06-16 10:50:59,904 INFO manager.MySQLManager: Preparing to use a MySQL streaming resultset.
2021-06-16 10:50:59,904 INFO tool.CodeGenTool: Beginning code generation
2021-06-16 10:51:00,123 INFO manager.SqlManager: Executing SQL statement: SELECT t.* FROM `guestbook` AS t LIMIT 1
2021-06-16 10:51:00,140 INFO manager.SqlManager: Executing SQL statement: SELECT t.* FROM `guestbook` AS t LIMIT 1
2021-06-16 10:51:00,145 INFO orm.CompilationManager: HADOOP_MAPRED_HOME is /usr/local/hadoop
Note: /tmp/sqoop-root/compile/a140820b5a43ac4421f0b91972226998/guestbook.java uses or overrides a deprecated API.
Note: Recompile with -Xlint:deprecation for details.
2021-06-16 10:51:00,968 INFO orm.CompilationManager: Writing jar file: /tmp/sqoop-root/compile/a140820b5a43ac4421f0b91972226998/guestbook.jar
2021-06-16 10:51:00,978 WARN manager.MySQLManager: It looks like you are importing from mysql.
2021-06-16 10:51:00,978 WARN manager.MySQLManager: This transfer can be faster! Use the --direct
2021-06-16 10:51:00,978 WARN manager.MySQLManager: option to exercise a MySQL-specific fast path.
2021-06-16 10:51:00,978 INFO manager.MySQLManager: Setting zero DATETIME behavior to convertToNull (mysql)
2021-06-16 10:51:00,981 INFO mapreduce.ImportJobBase: Beginning import of guestbook
2021-06-16 10:51:00,981 INFO Configuration.deprecation: mapred.job.tracker is deprecated. Instead, use mapreduce.jobtracker.address
2021-06-16 10:51:01,063 INFO Configuration.deprecation: mapred.jar is deprecated. Instead, use mapreduce.job.jar
2021-06-16 10:51:01,503 INFO Configuration.deprecation: mapred.map.tasks is deprecated. Instead, use mapreduce.job.maps
2021-06-16 10:51:01,579 INFO client.DefaultNoHARMFailoverProxyProvider: Connecting to ResourceManager at aidw-001/1.209.179.131:8032
2021-06-16 10:51:01,860 INFO mapreduce.JobResourceUploader: Disabling Erasure Coding for path: /tmp/hadoop-yarn/staging/root/.staging/job_1623806481074_0006
2021-06-16 10:51:04,551 INFO db.DBInputFormat: Using read commited transaction isolation
2021-06-16 10:51:04,552 INFO db.DataDrivenDBInputFormat: BoundingValsQuery: SELECT MIN(`idx`), MAX(`idx`) FROM `guestbook`
2021-06-16 10:51:04,604 INFO db.IntegerSplitter: Split size: 0; Num splits: 4 from: 1 to: 3
2021-06-16 10:51:04,734 INFO mapreduce.JobSubmitter: number of splits:3
2021-06-16 10:51:04,875 INFO mapreduce.JobSubmitter: Submitting tokens for job: job_1623806481074_0006
2021-06-16 10:51:04,875 INFO mapreduce.JobSubmitter: Executing with tokens: []
2021-06-16 10:51:05,055 INFO conf.Configuration: resource-types.xml not found
2021-06-16 10:51:05,055 INFO resource.ResourceUtils: Unable to find 'resource-types.xml'.
2021-06-16 10:51:05,104 INFO impl.YarnClientImpl: Submitted application application_1623806481074_0006
2021-06-16 10:51:05,130 INFO mapreduce.Job: The url to track the job: http://aidw-001:8088/proxy/application_1623806481074_0006/
2021-06-16 10:51:05,131 INFO mapreduce.Job: Running job: job_1623806481074_0006
2021-06-16 10:51:10,184 INFO mapreduce.Job: Job job_1623806481074_0006 running in uber mode : false
2021-06-16 10:51:10,187 INFO mapreduce.Job:  map 0% reduce 0%
2021-06-16 10:51:14,249 INFO mapreduce.Job:  map 100% reduce 0%
2021-06-16 10:51:14,262 INFO mapreduce.Job: Job job_1623806481074_0006 completed successfully
2021-06-16 10:51:14,322 INFO mapreduce.Job: Counters: 33
        File System Counters
                FILE: Number of bytes read=0
                FILE: Number of bytes written=818763
                FILE: Number of read operations=0
                FILE: Number of large read operations=0
                FILE: Number of write operations=0
                HDFS: Number of bytes read=301
                HDFS: Number of bytes written=177
                HDFS: Number of read operations=18
                HDFS: Number of large read operations=0
                HDFS: Number of write operations=6
                HDFS: Number of bytes read erasure-coded=0
        Job Counters
                Launched map tasks=3
                Other local map tasks=3
                Total time spent by all maps in occupied slots (ms)=6008
                Total time spent by all reduces in occupied slots (ms)=0
                Total time spent by all map tasks (ms)=6008
                Total vcore-milliseconds taken by all map tasks=6008
                Total megabyte-milliseconds taken by all map tasks=6152192
        Map-Reduce Framework
                Map input records=3
                Map output records=3
                Input split bytes=301
                Spilled Records=0
                Failed Shuffles=0
                Merged Map outputs=0
                GC time elapsed (ms)=126
                CPU time spent (ms)=2520
                Physical memory (bytes) snapshot=855822336
                Virtual memory (bytes) snapshot=7685423104
                Total committed heap usage (bytes)=1192755200
                Peak Map Physical memory (bytes)=296103936
                Peak Map Virtual memory (bytes)=2564374528
        File Input Format Counters
                Bytes Read=0
        File Output Format Counters
                Bytes Written=177
2021-06-16 10:51:14,326 INFO mapreduce.ImportJobBase: Transferred 177 bytes in 12.8158 seconds (13.8111 bytes/sec)
2021-06-16 10:51:14,328 INFO mapreduce.ImportJobBase: Retrieved 3 records.
```

#### 7) hdfs에 저장된 guestbook table 확인
```bash
(base) root@aidw-001:/opt/sqoop-1.4.7/lib# hdfs dfs -cat /user/root/guestbook/part-m-00000
1,kim,kim@naver.com,1234,방명록,2021-06-04 15:10:21.0
(base) root@aidw-001:/opt/sqoop-1.4.7/lib# hdfs dfs -cat /user/root/guestbook/part-m-00001
2,kim2,kim2@naver.com,3333,방명록2,2021-06-14 17:01:04.0
(base) root@aidw-001:/opt/sqoop-1.4.7/lib# hdfs dfs -cat /user/root/guestbook/part-m-00002
3,kim3,kim3@naver.com,3333,방명록3,2021-06-14 17:01:17.0
```

#### 8) mysql에 있는 web database 의 guestbook table를 hive로 이동.
```bash

(base) root@aidw-001:/opt/sqoop-1.4.7/lib# sqoop import --connect 'jdbc:mysql://aidw-001:3306/web?useSSL=false&allowPublicKeyRetrieval=true' --username user --password 1234 --table guestbook --hive-database test_web --hive-import
Warning: /opt/sqoop-1.4.7/../hbase does not exist! HBase imports will fail.
Please set $HBASE_HOME to the root of your HBase installation.
Warning: /opt/sqoop-1.4.7/../hcatalog does not exist! HCatalog jobs will fail.
Please set $HCAT_HOME to the root of your HCatalog installation.
Warning: /opt/sqoop-1.4.7/../accumulo does not exist! Accumulo imports will fail.
Please set $ACCUMULO_HOME to the root of your Accumulo installation.
Warning: /opt/sqoop-1.4.7/../zookeeper does not exist! Accumulo imports will fail.
Please set $ZOOKEEPER_HOME to the root of your Zookeeper installation.
2021-06-16 10:31:56,071 INFO sqoop.Sqoop: Running Sqoop version: 1.4.7
2021-06-16 10:31:56,127 WARN tool.BaseSqoopTool: Setting your password on the command-line is insecure. Consider using -P instead.
2021-06-16 10:31:56,127 INFO tool.BaseSqoopTool: Using Hive-specific delimiters for output. You can override
2021-06-16 10:31:56,127 INFO tool.BaseSqoopTool: delimiters with --fields-terminated-by, etc.
2021-06-16 10:31:56,194 INFO manager.MySQLManager: Preparing to use a MySQL streaming resultset.
2021-06-16 10:31:56,199 INFO tool.CodeGenTool: Beginning code generation
2021-06-16 10:31:56,414 INFO manager.SqlManager: Executing SQL statement: SELECT t.* FROM `guestbook` AS t LIMIT 1
2021-06-16 10:31:56,430 INFO manager.SqlManager: Executing SQL statement: SELECT t.* FROM `guestbook` AS t LIMIT 1
2021-06-16 10:31:56,436 INFO orm.CompilationManager: HADOOP_MAPRED_HOME is /usr/local/hadoop
Note: /tmp/sqoop-root/compile/b8abb9d7c05fa79a89405fbd71105254/guestbook.java uses or overrides a deprecated API.
Note: Recompile with -Xlint:deprecation for details.
2021-06-16 10:31:57,270 INFO orm.CompilationManager: Writing jar file: /tmp/sqoop-root/compile/b8abb9d7c05fa79a89405fbd71105254/guestbook.jar
2021-06-16 10:31:57,279 WARN manager.MySQLManager: It looks like you are importing from mysql.
2021-06-16 10:31:57,279 WARN manager.MySQLManager: This transfer can be faster! Use the --direct
2021-06-16 10:31:57,279 WARN manager.MySQLManager: option to exercise a MySQL-specific fast path.
2021-06-16 10:31:57,279 INFO manager.MySQLManager: Setting zero DATETIME behavior to convertToNull (mysql)
2021-06-16 10:31:57,283 INFO mapreduce.ImportJobBase: Beginning import of guestbook
2021-06-16 10:31:57,283 INFO Configuration.deprecation: mapred.job.tracker is deprecated. Instead, use mapreduce.jobtracker.address
2021-06-16 10:31:57,363 INFO Configuration.deprecation: mapred.jar is deprecated. Instead, use mapreduce.job.jar
2021-06-16 10:31:57,784 INFO Configuration.deprecation: mapred.map.tasks is deprecated. Instead, use mapreduce.job.maps
2021-06-16 10:31:57,853 INFO client.DefaultNoHARMFailoverProxyProvider: Connecting to ResourceManager at aidw-001/1.209.179.131:8032
2021-06-16 10:31:58,132 INFO mapreduce.JobResourceUploader: Disabling Erasure Coding for path: /tmp/hadoop-yarn/staging/root/.staging/job_1623806481074_0005
2021-06-16 10:32:00,861 INFO db.DBInputFormat: Using read commited transaction isolation
2021-06-16 10:32:00,861 INFO db.DataDrivenDBInputFormat: BoundingValsQuery: SELECT MIN(`idx`), MAX(`idx`) FROM `guestbook`
2021-06-16 10:32:00,865 INFO db.IntegerSplitter: Split size: 0; Num splits: 4 from: 1 to: 3
2021-06-16 10:32:00,988 INFO mapreduce.JobSubmitter: number of splits:3
2021-06-16 10:32:01,246 INFO mapreduce.JobSubmitter: Submitting tokens for job: job_1623806481074_0005
2021-06-16 10:32:01,247 INFO mapreduce.JobSubmitter: Executing with tokens: []
2021-06-16 10:32:01,425 INFO conf.Configuration: resource-types.xml not found
2021-06-16 10:32:01,425 INFO resource.ResourceUtils: Unable to find 'resource-types.xml'.
2021-06-16 10:32:01,477 INFO impl.YarnClientImpl: Submitted application application_1623806481074_0005
2021-06-16 10:32:01,503 INFO mapreduce.Job: The url to track the job: http://aidw-001:8088/proxy/application_1623806481074_0005/
2021-06-16 10:32:01,504 INFO mapreduce.Job: Running job: job_1623806481074_0005
2021-06-16 10:32:06,600 INFO mapreduce.Job: Job job_1623806481074_0005 running in uber mode : false
2021-06-16 10:32:06,602 INFO mapreduce.Job:  map 0% reduce 0%
2021-06-16 10:32:10,684 INFO mapreduce.Job:  map 100% reduce 0%
2021-06-16 10:32:10,698 INFO mapreduce.Job: Job job_1623806481074_0005 completed successfully
2021-06-16 10:32:10,816 INFO mapreduce.Job: Counters: 33
        File System Counters
                FILE: Number of bytes read=0
                FILE: Number of bytes written=818763
                FILE: Number of read operations=0
                FILE: Number of large read operations=0
                FILE: Number of write operations=0
                HDFS: Number of bytes read=301
                HDFS: Number of bytes written=177
                HDFS: Number of read operations=18
                HDFS: Number of large read operations=0
                HDFS: Number of write operations=6
                HDFS: Number of bytes read erasure-coded=0
        Job Counters
                Launched map tasks=3
                Other local map tasks=3
                Total time spent by all maps in occupied slots (ms)=6198
                Total time spent by all reduces in occupied slots (ms)=0
                Total time spent by all map tasks (ms)=6198
                Total vcore-milliseconds taken by all map tasks=6198
                Total megabyte-milliseconds taken by all map tasks=6346752
        Map-Reduce Framework
                Map input records=3
                Map output records=3
                Input split bytes=301
                Spilled Records=0
                Failed Shuffles=0
                Merged Map outputs=0
                GC time elapsed (ms)=139
                CPU time spent (ms)=2700
                Physical memory (bytes) snapshot=844513280
                Virtual memory (bytes) snapshot=7683846144
                Total committed heap usage (bytes)=1096286208
                Peak Map Physical memory (bytes)=294764544
                Peak Map Virtual memory (bytes)=2564734976
        File Input Format Counters
                Bytes Read=0
        File Output Format Counters
                Bytes Written=177
2021-06-16 10:32:10,822 INFO mapreduce.ImportJobBase: Transferred 177 bytes in 13.0299 seconds (13.5841 bytes/sec)
2021-06-16 10:32:10,826 INFO mapreduce.ImportJobBase: Retrieved 3 records.
2021-06-16 10:32:10,826 INFO mapreduce.ImportJobBase: Publishing Hive/Hcat import job data to Listeners for table guestbook
2021-06-16 10:32:10,841 INFO manager.SqlManager: Executing SQL statement: SELECT t.* FROM `guestbook` AS t LIMIT 1
2021-06-16 10:32:10,845 WARN hive.TableDefWriter: Column post_date had to be cast to a less precise type in Hive
2021-06-16 10:32:10,847 INFO hive.HiveImport: Loading uploaded data into Hive
2021-06-16 10:32:10,852 INFO conf.HiveConf: Found configuration file file:/opt/hive-3.1.2/conf/hive-site.xml
2021-06-16 10:32:10,953 WARN conf.HiveConf: HiveConf of name hive.metastore.local does not exist
2021-06-16 10:32:11,756 INFO hive.HiveImport: SLF4J: Class path contains multiple SLF4J bindings.
2021-06-16 10:32:11,756 INFO hive.HiveImport: SLF4J: Found binding in [jar:file:/opt/hive-3.1.2/lib/log4j-slf4j-impl-2.10.0.jar!/org/slf4j/impl/StaticLoggerBinder.class]
2021-06-16 10:32:11,756 INFO hive.HiveImport: SLF4J: Found binding in [jar:file:/usr/local/hadoop/share/hadoop/common/lib/slf4j-log4j12-1.7.25.jar!/org/slf4j/impl/StaticLoggerBinder.class]
2021-06-16 10:32:11,756 INFO hive.HiveImport: SLF4J: See http://www.slf4j.org/codes.html#multiple_bindings for an explanation.
2021-06-16 10:32:11,760 INFO hive.HiveImport: SLF4J: Actual binding is of type [org.apache.logging.slf4j.Log4jLoggerFactory]
2021-06-16 10:32:13,478 INFO hive.HiveImport: Hive Session ID = 7385b39c-4361-4889-878c-b5d1782ed9e3
2021-06-16 10:32:13,518 INFO hive.HiveImport:
2021-06-16 10:32:13,519 INFO hive.HiveImport: Logging initialized using configuration in jar:file:/opt/hive-3.1.2/lib/hive-common-3.1.2.jar!/hive-log4j2.properties Async: true
2021-06-16 10:32:18,323 INFO hive.HiveImport: Hive Session ID = 7f71a4b8-692d-4bc0-88a6-5d20a8c5e73c
2021-06-16 10:32:19,736 INFO hive.HiveImport: OK
2021-06-16 10:32:19,736 INFO hive.HiveImport: Time taken: 1.377 seconds
2021-06-16 10:32:19,968 INFO hive.HiveImport: Loading data to table test_web.guestbook
2021-06-16 10:32:20,351 INFO hive.HiveImport: OK
2021-06-16 10:32:20,351 INFO hive.HiveImport: Time taken: 0.613 seconds
2021-06-16 10:32:20,761 INFO hive.HiveImport: Hive import complete.
2021-06-16 10:32:20,771 INFO hive.HiveImport: Export directory is contains the _SUCCESS file only, removing the directory.
```

#### 9) hive에 정상적으로 적용 되었는지 확인
```bash
hive> use test_web;
OK
Time taken: 0.049 seconds
hive> show tables;
OK
guestbook
Time taken: 0.113 seconds, Fetched: 1 row(s)
hive> select * from guestbook;
OK
1       kim     kim@naver.com   1234    방명록  2021-06-04 15:10:21.0
2       kim2    kim2@naver.com  3333    방명록2 2021-06-14 17:01:04.0
3       kim3    kim3@naver.com  3333    방명록3 2021-06-14 17:01:17.0
Time taken: 1.323 seconds, Fetched: 3 row(s)
```
