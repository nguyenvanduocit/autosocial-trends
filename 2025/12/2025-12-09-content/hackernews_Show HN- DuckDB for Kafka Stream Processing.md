---
source: hackernews
title: Show HN: DuckDB for Kafka Stream Processing
url: https://sql-flow.com/docs/tutorials/intro/
date: 2025-12-09
---

[Skip to main content](#__docusaurus_skipToContent_fallback)

[![My Site Logo](/img/sqlflow-small.png)![My Site Logo](/img/sqlflow-small.png)

**SQLFlow Documentation**](/)[Docs](/docs/category/introduction)[Tutorials](/docs/category/tutorials/)

[GitHub](https://github.com/turbolytics/sql-flow)

* [Introduction](/docs/category/introduction)
* [Operations](/docs/category/operations)
* [Tutorials](/docs/category/tutorials)

  + [Quickstart](/docs/tutorials/intro)
  + [Learning SQLFlow Using the Bluesky Firehose](/docs/tutorials/bluesky-firehose)
  + [Stream 70k Rows per second to Clickhouse using SQLFlow](/docs/tutorials/clickhouse-sink)
  + [🦆 Sink Kafka to DuckLake with SQLFlow](/docs/tutorials/ducklake-sink)
  + [Streaming to Iceberg using SQLFlow](/docs/tutorials/iceberg-sink)
  + [Integrating SQLFlow with MotherDuck](/docs/tutorials/motherduck-sink)
  + [SQLFlow: Join Kafka With Postgres To Enrich Real time Data Streams](/docs/tutorials/postgres-join)
  + [Using SQLFlow to Sink 5000 Rows / Second from Kafka To Postgres](/docs/tutorials/postgres-sink)
  + [Stream Data from Kafka to S3 in Parquet Format using SQLFlow](/docs/tutorials/s3-parquet-sink)
* [FAQ](/docs/faq)

* [Tutorials](/docs/category/tutorials)
* Quickstart

On this page

# Quickstart

Create a stream processor that reads data from Kafka **in less than 5 minutes**.

## Getting Started[​](#getting-started "Direct link to Getting Started")

Get started by **running a stream processor that executes SQL against a kafka stream and writes the output to the console**.

### What you'll need[​](#what-youll-need "Direct link to What you'll need")

* Docker
* A copy of turbolytics/sql-flow cloned on your local machine (<https://github.com/turbolytics/sql-flow>)
* turbolytics/sql-flow Python dependencies installed

```
cd path/to/turbolytics/sql-flow/github/repo && pip install -r requirements.txt
```

* The turbolytics/sql-flow docker image

```
docker pull turbolytics/sql-flow:latest
```

* Kafka running on your local machine

```
cd path/to/turbolytics/sql-flow/github/repo && docker-compose -f dev/kafka-single.yml up -d
```

## Test the SQLFlow configuration file[​](#test-the-sqlflow-configuration-file "Direct link to Test the SQLFlow configuration file")

SQLFlow ships with cli support to test a stream configuration against any fixture file of test data. The goal is to support testing and linting of a configuration file before executing in a stream environment.

Run the invoke command to test the configuration file against a set of test data:

```
docker run -v $(pwd)/dev:/tmp/conf -v /tmp/sqlflow:/tmp/sqlflow turbolytics/sql-flow:latest dev invoke /tmp/conf/config/examples/basic.agg.mem.yml /tmp/conf/fixtures/simple.json
```

The following output should show:

```
[{'city': 'New York', 'city_count': 28672}, {'city': 'Baltimore', 'city_count': 28672}]
```

## Run SQLFlow against a Kafka stream[​](#run-sqlflow-against-a-kafka-stream "Direct link to Run SQLFlow against a Kafka stream")

This section runs SQLFlow as a stream processor that reads data from a Kafka topic and writes the output to the console. SQLFow runs as a daemon and will continuously read data from kafka, execute the SQL and write the output to the console.

* Publish test messages to the Kafka topic

```
python3 cmd/publish-test-data.py --num-messages=10000 --topic="input-simple-agg-mem"
```

* Start the Kafka Console Consumer, to view the SQLFlow output

```
docker exec -it kafka1 kafka-console-consumer --bootstrap-server=kafka1:9092 --topic=output-simple-agg-mem
```

* Start SQLFlow

```
docker run -v $(pwd)/dev:/tmp/conf -v /tmp/sqlflow:/tmp/sqlflow -e SQLFLOW_KAFKA_BROKERS=host.docker.internal:29092 turbolytics/sql-flow:latest run /tmp/conf/config/examples/basic.agg.mem.yml --max-msgs-to-process=10000
```

The following output should begin to show in the kafka console consumer:

```
...
...
{"city":"San Francisco504","city_count":1}
{"city":"San Francisco735","city_count":1}
{"city":"San Francisco533","city_count":1}
{"city":"San Francisco556","city_count":1}
...
```

[Previous

Tutorials](/docs/category/tutorials)[Next

Learning SQLFlow Using the Bluesky Firehose](/docs/tutorials/bluesky-firehose)

* [Getting Started](#getting-started)
  + [What you'll need](#what-youll-need)
* [Test the SQLFlow configuration file](#test-the-sqlflow-configuration-file)
* [Run SQLFlow against a Kafka stream](#run-sqlflow-against-a-kafka-stream)

Docs

* [Tutorials](/docs/tutorials/intro)

Community

More

* [GitHub](https://github.com/turbolytics/sql-flow)

Copyright © 2025 Turbolytics, LLC. Built with Docusaurus.
