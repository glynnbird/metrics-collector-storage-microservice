# Metrics Collector Storage Microservice

This is a microservice that subscribes to a queue or pubsub channel hosted on 

* Redis 
* RabbitMQ
* Apache Kafka 

and writes the data to either

* Cloudant/CouchDB
* MongoDB
* ElasticSearch

It uses the bulk write APIs of each database for maximum efficiency.

Environment variables are used to determine which message hub and which database to use.

## Installation

Clone this repository and then run

```
npm install
```

## Usage

Set your environment variables and start up the app e.g.

```
export QUEUE_TYPE=redis_queue
export DATABASE_TYPE=cloudant
node app.js
```

When running in Bluemix, the service credentials will be discovered automatically from the Bluemix VCAP_SERVICES environment variable, but you must still specify the QUEUE_TYPE and DATABASE_TYPE you want using custom environment variabless.

## Environment variables

### QUEUE_TYPE

One of 

* redis_queue - A Redis list data structure
* redis_pubsub - A Redis PubSub channel
* rabbit_queue - A RabbitMQ PUSH/WORKER queue
* rabbit_pubsub - A RabbitMQ PUBLISH/SUBSCRIBE channel
* kafka - An Apache Kafka topic
* null - default (does nothing)

### QUEUE_NAME

The name of the queue/channel that is subscribed to. If omitted, it takes the following values for each of the queue types:

1. stdout - n/a
2. redis_queue - mcqueue
3. redis_pubsub - mcpubsub
4. rabbit_queue - mcqueue
5. rabbit_pubsub - mcpubsub
6. kafka - mcqueue

### DATABASE_TYPE

* stdout - (default) writes the data to the console only
* cloudant - An IBM Cloudant NoSQL database
* mongodb - A MongoDB database
* elasticsearch - An ElasticSearch engine

### DATABASE_NAME

The name of the Cloudant database to write to. Also defines to the MongoDB database and collection, and the ElasticSearch database and type. Defaults to "mc".

### BUFFER_SIZE

The number of documents to be written in one operation. Defaults to 50.

### PARALLELISM

The number if parallel writes that the are made to the database. Defaults to 5.

### VCAP_SERVICES

`VCAP_SERVICES` is created for you by the Bluemix Cloud Foundry service. It defines the credentials of the attached services that this app can connect to. 



