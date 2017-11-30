# Spark Stemming Fork

A fork to update to Spark 2.2 and operate on Array(StringType) input rather than only string input.  That is, this fork fixes:

```
java.lang.IllegalArgumentException: requirement failed: Input type must be string type but got ArrayType(StringType,true)
```

Maven and SBT probably won't work as described in the README below but you should be able to compile this locally.

== original README ==

[Snowball](http://snowballstem.org/) is a small string processing language
designed for creating stemming algorithms for use in Information Retrieval.
This package allows to use it as a part of [Spark ML
Pipeline](https://spark.apache.org/docs/latest/ml-guide.html) API.

## Linking

Link against this library using SBT:

```
libraryDependencies += "master" %% "spark-stemming" % "0.1.2"
```

Using Maven:

```xml
<dependency>
    <groupId>master</groupId>
    <artifactId>spark-stemming_2.10</artifactId>
    <version>0.1.2</version>
</dependency>
```

Or include it when starting the Spark shell:

```
$ bin/spark-shell --packages master:spark-stemming_2.10:0.1.2
```

## Features

Currently implemented algorithms:

* English
* English (Porter)
* Romance stemmers:
 * French
 * Spanish
 * Portuguese
 * Italian
 * Romanian
* Germanic stemmers:
 * German
 * Dutch
* Scandinavian stemmers:
 * Swedish
 * Norwegian (Bokmål)
 * Danish
* Russian
* Finnish
* Greek

More details are on the [Snowball stemming algorithms](http://snowballstem.org/algorithms/) page.

## Usage

`Stemmer`
[Transformer](https://spark.apache.org/docs/latest/ml-guide.html#transformers)
can be used directly or as a part of ML
[Pipeline](https://spark.apache.org/docs/latest/ml-guide.html#pipeline). In
particular, it is nicely combined with
[Tokenizer](https://spark.apache.org/docs/latest/ml-features.html#tokenizer).

```scala
import org.apache.spark.mllib.feature.Stemmer

val data = sqlContext
  .createDataFrame(Seq(("мама", 1), ("мыла", 2), ("раму", 3)))
  .toDF("word", "id")

val stemmed = new Stemmer()
  .setInputCol("word")
  .setOutputCol("stemmed")
  .setLanguage("Russian")
  .transform(data)

stemmed.show
```
