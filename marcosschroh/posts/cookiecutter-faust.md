<!--
.. title: Cookiecutter Faust
.. slug: cookiecutter-faust
.. date: 2019-06-07 12:40:51 UTC+02:00
.. tags: python,faust,data streaming
.. category: data streaming
.. link: 
.. description: 
.. type: text
-->

Businesses collect large amounts of data, and data experts can extract actionable insights and learn from it. Because we can leverage data in real time, employing data streaming and processing, insights can be discovered almost instantly.

Tools like [Kafka Streams](https://kafka.apache.org/documentation/streams/), [Apache Spark](https://spark.apache.org/), and [Flink](https://flink.apache.org/) are used for this propouse, but mainly with support for `Java` and `Scala`.

Recently, a new framework was born for the Python world: [Faust](https://faust.readthedocs.io/en/latest/).

> Faust is a stream processing library, porting the ideas from Kafka Streams to Python.
It is used to build high performance distributed systems and real-time data pipelines that process billions of events every day.
Faust provides both stream processing and event processing, sharing similarity with tools such as Kafka Streams, Apache Spark/Storm/Samza/Flink,
It does not use a DSL, it’s just Python! This means you can use all your favorite Python libraries when stream processing: NumPy, PyTorch, Pandas, NLTK, Django, Flask, SQLAlchemy, ++
Faust requires Python 3.6 or later for the new async/await syntax, and variable type annotations.

In order to use a data streaming technology, we need to set up a broker, for example [kafka](https://kafka.apache.org/), and `kafka` needs [zookeeper](https://zookeeper.apache.org/), and this is when we start struggling a bit because the different parts have to be installed and configured in order to play together. Indeed, we can go further, and use services like `Schema Registry` and `Rocks DB` to make more robust our stack, and again, we need to spend time configuring them.  

So, I have created a small project called [cookiecutter-faust](https://github.com/marcosschroh/cookiecutter-faust): `A Cookiecutter template for creating Faust projects quickly`, means that all the necessary services are pre-configured and the project skeleton is generated for you.

The requirements are `cookiecutter`, `Docker` and `Docker Compose`.

### Features

* For Faust 1.5.4
* Python 3.6 and 3.7
* Docker and docker-compose support
* Useful commands included in Makefile
* Project skeleton is defined as a medium/large project according to faust layout
* The `setup.py` has the entrypoint in order to solve the entrypoint problem in Faust
* Include a `settings.py` as `Django`
* Include an App example with tests

### Usage

Is super easy. First, we need to install `coockicutter`. 

```sh
pip install "cookiecutter>=1.4.0"
```

Then, just run:

```sh
cookiecutter https://github.com/marcosschroh/cookiecutter-faust
```
and answer the prompts with your desired option:

```sh
project_name [My Awesome Faust Project]: super faust
project_slug [super_faust]:
description [My Awesome Faust Project!]:
long_description [My Awesome Faust Project!]:
author_name [Marcos Schroh]:
author_email [marcos-schroh@gmail.com]:
version [0.1.0]:
Select open_source_license:
1 - MIT
2 - BSD
3 - GPLv3
4 - Apache Software License 2.0
5 - Not open source
Choose from 1, 2, 3, 4, 5 (1, 2, 3, 4, 5) [1]:
use_pycharm [n]:
use_docker [n]: y
include_docker_compose [n]: y
include_page_view_tutorial [n]: y
worker_port [6066]:
kafka_server_environment_variable [KAFKA_BOOTSTRAP_SERVER]:
include_codec_example [y]:
Select faust_loglevel:
1 - CRITICAL
2 - ERROR
3 - WARNING
4 - INFO
5 - DEBUG
6 - NOTSET
Choose from 1, 2, 3, 4, 5, 6 (1, 2, 3, 4, 5, 6) [1]: 4
```

and now you are ready to start coding your Data Streaming application without spending time on configurations!

The full Documentation is [here](https://github.com/marcosschroh/cookiecutter-faust/blob/master/README.md)






