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

Bedrijven krijgen long som gegevens, en gegevens specialists kunnen extract informatie en leren uit het. Wants we kunnen leverenen gegevens in real tijd, applicatie `Data Streaming` en `Processing Tecnics`, kennis kun ogenblikkelijk ontdekt.

Frameworks zoals [Kafka Streams](https://kafka.apache.org/documentation/streams/), [Apache Spark](https://spark.apache.org/), en [Flink](https://flink.apache.org/) zijn gebruiken voor dit wit, maar special ondersteuning voor `Java` en `Scala`.

Kort geleden, een nieuwe framework hebt geboren voor Python: [Faust](https://faust.readthedocs.io/en/latest/).

> Faust is a stream processing library, porting the ideas from Kafka Streams to Python.
It is used to build high performance distributed systems and real-time data pipelines that process billions of events every day.
Faust provides both stream processing and event processing, sharing similarity with tools such as Kafka Streams, Apache Spark/Storm/Samza/Flink,
It does not use a DSL, it’s just Python! This means you can use all your favorite Python libraries when stream processing: NumPy, PyTorch, Pandas, NLTK, Django, Flask, SQLAlchemy, ++
Faust requires Python 3.6 or later for the new async/await syntax, and variable type annotations.

Als we bruiken een gegevens streaming technologie, we hebben en broker stellen nodig, bij voorbeeld[kafka](https://kafka.apache.org/), en `kafka` hebt [zookeeper](https://zookeeper.apache.org/) nodig, en this is when we start struggling a bit because the different parts have to be installed and configured in order to play together. Indeed, we can go further, and use services like `Schema Registry` and `Rocks DB` to make more robust our stack, and again, we need to spend time configuring them.  

Dus, Ik heb gecreëerd en kleine project heet [cookiecutter-faust](https://github.com/marcosschroh/cookiecutter-faust): `A Cookiecutter template for creating Faust projects quickly`, means that all the necessary services are pre-configured and the project skeleton is generated for you.

The requirements zijn `cookiecutter`, `Docker` en `Docker Compose`.

### Kenmerken

* Faust 1.5.4
* Python 3.6 and 3.7
* Docker en docker-compose ondersteuning
* Useful commands included in Makefile
* Project skeleton is defined as a medium/large project according to faust layout
* The `setup.py` has the entrypoint in order to solve the entrypoint problem in Faust
* Include a `settings.py` as `Django`
* Include an App example with tests

### Gebruiken

Is super makkelijk. Eerste, we hebben install `coockicutter` nodig. 

```sh
pip install "cookiecutter>=1.4.0"
```

Then, just run:

```sh
cookiecutter https://github.com/marcosschroh/cookiecutter-faust
```
antword de vragens met jouw opties:

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

En nu je bent klaar te coding jouw Data Streaming applicatie zonder spending tijd in configuratie!

De projectdocumentatie is [hier](https://github.com/marcosschroh/cookiecutter-faust/blob/master/README.md)






