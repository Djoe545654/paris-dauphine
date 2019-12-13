# Flink

This directory contains a docker-compose.yml file containing Flink.
Flink can be downloaded from : [https://www.apache.org/dyn/closer.lua/flink/flink-1.9.1/flink-1.9.1-bin-scala_2.11.tgz](flink.apache.org)
Version build with scala 2.12 does not support Flink REPL.

You can also run this command line to have a Flink REPL within a docker container :

```
$> docker run -it --rm  -v "$PWD":/usr/src/app -w /usr/src/app flink:1.9.1-scala_2.11 start-scala-shell.sh local
```

# Try it

## Word count

### Using the DataSet API :

```
val data = benv.readTextFile("/usr/src/app/data/lorem-ipsum.txt")

data
  .filter(_.nonEmpty)
  .flatMap(_.split(" "))
  .map((_, 1))
  .groupBy(0)
  .sum(1)
  .print

```

### Using the DataStream API :

```
val data = senv.readTextFile("/usr/src/app/data/lorem-ipsum.txt")

data
  .filter(_.nonEmpty)
  .flatMap(_.split(" "))
  .map((_, 1))
  .groupkeyBy(0)
  .sum(1)
  .print

senv.execute("Wordcount using DataStream API")

```

### Using SQL :

```
val source = senv.readTextFile("/usr/src/app/data/lorem-ipsum.txt")

stenv.registerDataStream("Words", source);

val result = stenv.sqlQuery("SELECT * FROM Words")

result.toAppendStream[String].print

stenv.execute("test""Wordcount using SQL")
```
