# How to use BenchBase to benchmark MySQL
## Dependencies

JAVA and maven environment as follow for using benchbase tool,

```shell
https://www.linuxcapable.com/how-to-install-oracle-java-17-on-ubuntu-linux/#Method-1-Install-Java-17-with-PPA-Method
https://www.digitalocean.com/community/tutorials/install-maven-linux-ubuntu and https://maven.apache.org/download.cgi
```

>> NOTE: It is required larger than java 17 for java version in codes.

## Installation

To modify the mysql connector version in `pom.xml`,

```shell
<dependencies>
    <dependency>
        <groupId>mysql</groupId>
        <artifactId>mysql-connector-java</artifactId>
        <version>8.0.31</version>
    </dependency>
</dependencies>
```

In my experiments, I use the mysql 8.0.31 version.

To clone and build BenchBase using the `mysql` profile,

```bash
git clone --depth 1 git@github.com:abcdabcd3899/benchbase.git
cd benchbase
./mvnw clean package -P mysql
mvn clean package -P mysql -DskipTests
```

reference: https://mariadb.com/resources/blog/how-to-benchmark-mariadb-mysql-using-java-connector/

This produces artifacts in the `target` folder, which can be extracted,

```bash
cd target
tar -xvf benchbase-mysql.tgz
cd benchbase-mysql
```

Inside this folder, you can run BenchBase. For example, to execute the `tpcc` benchmark,

```bash
# create database tpcc first.
java -jar benchbase.jar -b tpcc -c config/mysql/sample_tpcc_config.xml --create=true --load=true --execute=true
```

For composite benchmarks like `chbenchmark`, which require multiple schemas to be created and loaded, you can provide a comma separated list:
```bash
java -jar benchbase.jar -b tpcc,chbenchmark -c config/postgres/sample_chbenchmark_config.xml --create=true --load=true --execute=true
```


A full list of options can be displayed,

```bash
java -jar benchbase.jar -h
```

The following options are provided:

```text
usage: benchbase
 -b,--bench <arg>               [required] Benchmark class. Currently
                                supported: [tpcc, tpch, tatp, wikipedia,
                                resourcestresser, twitter, epinions, ycsb,
                                seats, auctionmark, chbenchmark, voter,
                                sibench, noop, smallbank, hyadapt, otmetrics]
 -c,--config <arg>              [required] Workload configuration file
    --clear <arg>               Clear all records in the database for this
                                benchmark
    --create <arg>              Initialize the database for this benchmark
 -d,--directory <arg>           Base directory for the result files,
                                default is current directory
    --dialects-export <arg>     Export benchmark SQL to a dialects file
    --execute <arg>             Execute the benchmark workload
 -h,--help                      Print this help
 -im,--interval-monitor <arg>   Throughput Monitoring Interval in
                                milliseconds
    --load <arg>                Load data using the benchmark's data
                                loader
 -s,--sample <arg>              Sampling window
```

---

## Description

Benchmarking is incredibly useful, yet endlessly painful. This benchmark suite is the result of a group of
PhDs/post-docs/professors getting together and combining their workloads/frameworks/experiences/efforts. We hope this
will save other people's time, and will provide an extensible platform, that can be grown in an open-source fashion.

BenchBase is a multi-threaded load generator. The framework is designed to be able to produce variable rate,
variable mixture load against any JDBC-enabled relational database. The framework also provides data collection
features, e.g., per-transaction-type latency and throughput logs.

The BenchBase framework has the following benchmarks:

* [AuctionMark](https://github.com/cmu-db/benchbase/wiki/AuctionMark)
* [CH-benCHmark](https://github.com/cmu-db/benchbase/wiki/CH-benCHmark)
* [Epinions.com](https://github.com/cmu-db/benchbase/wiki/epinions)
* hyadapt -- pending configuration files
* [NoOp](https://github.com/cmu-db/benchbase/wiki/NoOp)
* [OT-Metrics](https://github.com/cmu-db/benchbase/wiki/OT-Metrics)
* [Resource Stresser](https://github.com/cmu-db/benchbase/wiki/Resource-Stresser)
* [SEATS](https://github.com/cmu-db/benchbase/wiki/Seats)
* [SIBench](https://github.com/cmu-db/benchbase/wiki/SIBench)
* [SmallBank](https://github.com/cmu-db/benchbase/wiki/SmallBank)
* [TATP](https://github.com/cmu-db/benchbase/wiki/TATP)
* [TPC-C](https://github.com/cmu-db/benchbase/wiki/TPC-C)
* [TPC-H](https://github.com/cmu-db/benchbase/wiki/TPC-H)
* TPC-DS -- pending configuration files
* [Twitter](https://github.com/cmu-db/benchbase/wiki/Twitter)
* [Voter](https://github.com/cmu-db/benchbase/wiki/Voter)
* [Wikipedia](https://github.com/cmu-db/benchbase/wiki/Wikipedia)
* [YCSB](https://github.com/cmu-db/benchbase/wiki/YCSB)



