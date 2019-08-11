spark-flamegraph
================

[![Build Status](https://travis-ci.org/spektom/spark-flamegraph.svg?branch=master)](https://travis-ci.org/spektom/spark-flamegraph)

Easy CPU Profiling for [Apache Spark](https://spark.apache.org/) applications.

The script `spark-submit-flamegraph` is a wrapper around standard `spark-submit` that generates [Flame Graph](http://www.brendangregg.com/flamegraphs.html).

<img src="flamegraph.png" height="250px" />

## Supported Systems

 * Amazon EMR
 * Most Linux distributions
 * Mac (with [Homebrew](https://brew.sh/) installed)

## Prerequisites

The script is adapted for work in [Amazon EMR](https://aws.amazon.com/emr/).
Otherwise the following utilities must present on your system:

 * perl
 * python2.7 (or set `PYTHON` environment variable to the Python executabl)
 * pip (or set `PIP` environment variable to the pip utility)

## Running

```bash
wget -O /usr/local/bin/spark-submit-flamegraph \
  https://raw.githubusercontent.com/spektom/spark-flamegraph/master/spark-submit-flamegraph

chmod +x /usr/local/bin/spark-submit-flamegraph
```

Use `spark-submit-flamegraph` as a replacement for the `spark-submit` command.

## Configuration

To configure use the following environment variables:

| Environment Variable | Description  | Default value |
| -------------------- | ------------ | ------------- |
| `SPARK_CMD` | Spark command to run | spark-submit |
| `PYTHON` | Path to the Python executable | python2.7 |
| `PIP` | Path to the pip utility | pip |

For example, to profile Spark shell session set `SPARK_CMD` environment variable:

```bash
SPARK_CMD=spark-shell /usr/local/bin/spark-submit-flamegraph
```

## Details

The script does the following operations to make profiling Spark applications as easy as possible:

  * Downloads InfluxDB, and starts it on some random port.
  * Starts Spark application using original `spark-submit` command, with the StatsD profiler Jar in its classpath and with the configuration that tells it to report statistics back to the InfluxDB instance.
  * After running Spark application, queries all the reported metrics from the InfluxDB instance.
  * Run a script that generates the .SVG file.
  * Stops the InfluxDB instance.

