# Logstash

## 특징

- 무료 오픈소스
- 다양한 소스에서 데이터를 수집하여 변환한 후 자주 사용하는 저장소로 전달
- 형식이나 복잡성과 관계 없이 데이터를 동적으로 수집, 전환, 전송
- grok을 이용해 비구조적 데이터에서 구조를 도출하여 IP 주소에서 위치 정보 좌표를 해독하고, 민감한 필드를 익명화하거나 제외시키며, 전반적인 처리를 손쉽게 가능
- 로그, 메트릭, 웹 애플리케이션, 데이터 저장소 및 다양한 AWS 서비스에서 모두 지속적으로 스트리밍되는 방식으로 손쉽게 수집

## 지원하는 데이터 소스 - Plugin	(Description)

- Plugin	(Description)
- azure_event_hubs	(Receives events from Azure Event Hubs)
- beats	(Receives events from the Elastic Beats framework)
- cloudwatch	(Pulls events from the Amazon Web Services CloudWatch API)
- couchdb_changes	(Streams events from CouchDB’s _changes URI)
- dead_letter_queue	(read events from Logstash’s dead letter queue)
- elasticsearch	(Reads query results from an Elasticsearch cluster)
- exec	(Captures the output of a shell command as an event)
- file	(Streams events from files)
- ganglia	(Reads Ganglia packets over UDP)
- gelf	(Reads GELF-format messages from Graylog2 as events)
- generator	(Generates random log events for test purposes)
- github	(Reads events from a GitHub webhook)
- google_cloud_storage	(Extract events from files in a Google Cloud Storage bucket)
- google_pubsub	(Consume events from a Google Cloud PubSub service)
- graphite	(Reads metrics from the graphite tool)
- heartbeat	(Generates heartbeat events for testing)
- http	(Receives events over HTTP or HTTPS)
- http_poller	(Decodes the output of an HTTP API into events)
- imap	(Reads mail from an IMAP server)
- irc	(Reads events from an IRC server)
- java_generator	(Generates synthetic log events)
- java_stdin	(Reads events from standard input)
- jdbc	(Creates events from JDBC data)
- jms	(Reads events from a Jms Broker)
- jmx	(Retrieves metrics from remote Java applications over JMX)
- kafka	(Reads events from a Kafka topic)
- kinesis	(Receives events through an AWS Kinesis stream)
- log4j	(Reads events over a TCP socket from a Log4j SocketAppender object)
- lumberjack	(Receives events using the Lumberjack protocl)
- meetup	(Captures the output of command line tools as an event)
- pipe	(Streams events from a long-running command pipe)
- puppet_facter	(Receives facts from a Puppet server)
- rabbitmq	(Pulls events from a RabbitMQ exchange)
- redis	(Reads events from a Redis instance)
- relp	(Receives RELP events over a TCP socket)
- rss	(Captures the output of command line tools as an event)
- s3	(Streams events from files in a S3 bucket)
- s3-(sns-sqs	Reads logs from AWS S3 buckets using sqs)
- salesforce	(Creates events based on a Salesforce SOQL query)
- snmp	(Polls network devices using Simple Network Management Protocol (SNMP))
- snmptrap	(Creates events based on SNMP trap messages)
- sqlite	(Creates events based on rows in an SQLite database)
- sqs	(Pulls events from an Amazon Web Services Simple Queue Service queue)
- stdin	(Reads events from standard input)
- stomp	(Creates events received with the STOMP protocol)
- syslog	(Reads syslog messages as events)
- tcp	(Reads events from a TCP socket)
- twitter	(Reads events from the Twitter Streaming API)
- udp	(Reads events over UDP)
- unix	(Reads events over a UNIX socket)
- varnishlog	(Reads from the varnish cache shared memory log)
- websocket	(Reads events from a websocket)
- wmi	(Creates events based on the results of a WMI query)
- xmpp	(Receives events over the XMPP/Jabber protocol)


## 타켓 저장소

- kafka	(Writes events to a Kafka topic)
- webhdfs	(Sends Logstash events to HDFS using the webhdfs REST API)
- logstash-hdfs (비공식)

- boundary	(Sends annotations to Boundary based on Logstash events)
- circonus	(Sends annotations to Circonus based on Logstash events)
- cloudwatch	(Aggregates and sends metric data to AWS CloudWatch)
- csv	(Writes events to disk in a delimited format)
- datadog	(Sends events to DataDogHQ based on Logstash events)
- datadog_metrics	(Sends metrics to DataDogHQ based on Logstash events)
- elastic_app_search	(Sends events to the Elastic App Search solution)
- elasticsearch	(Stores logs in Elasticsearch)
- email	(Sends email to a specified address when output is received)
- exec	(Runs a command for a matching event)
- file	(Writes events to files on disk)
- ganglia	(Writes metrics to Ganglia’s gmond)
- gelf	(Generates GELF formatted output for Graylog2)
- google_bigquery	(Writes events to Google BigQuery)
- google_cloud_storage	(Uploads log events to Google Cloud Storage)
- google_pubsub	(Uploads log events to Google Cloud Pubsub)
- graphite	(Writes metrics to Graphite)
- graphtastic	(Sends metric data on Windows)
- http	(Sends events to a generic HTTP or HTTPS endpoint)
- influxdb	(Writes metrics to InfluxDB)
- irc	(Writes events to IRC)
- sink	(Discards any events received)
- java_stdout	(Prints events to the STDOUT of the shell)
- juggernaut	(Pushes messages to the Juggernaut websockets server)
- librato	(Sends metrics, annotations, and alerts to Librato based on Logstash events)
- loggly	(Ships logs to Loggly)
- lumberjack	(Sends events using the lumberjack protocol)
- metriccatcher	(Writes metrics to MetricCatcher)
- mongodb	(Writes events to MongoDB)
- nagios	(Sends passive check results to Nagios)
- nagios_nsca	(Sends passive check results to Nagios using the NSCA protocol)
- opentsdb	(Writes metrics to OpenTSDB)
- pagerduty	(Sends notifications based on preconfigured services and escalation policies)
- pipe	(Pipes events to another program’s standard input)
- rabbitmq	(Pushes events to a RabbitMQ exchange)
- redis	(Sends events to a Redis queue using the RPUSH command)
- redmine	(Creates tickets using the Redmine API)
- riak	(Writes events to the Riak distributed key/value store)
- riemann	(Sends metrics to Riemann)
- s3	(Sends Logstash events to the Amazon Simple Storage Service)
- sns	(Sends events to Amazon’s Simple Notification Service)
- solr_http	(Stores and indexes logs in Solr)
- sqs	(Pushes events to an Amazon Web Services Simple Queue Service queue)
- statsd	(Sends metrics using the statsd network daemon)
- stdout	(Prints events to the standard output)
- stomp	(Writes events using the STOMP protocol)
- syslog	(Sends events to a syslog server)
- tcp	(Writes events over a TCP socket)
- timber	(Sends events to the Timber.io logging service)
- udp	(Sends events over UDP)
- websocket	(Publishes messages to a websocket)
- xmpp	(Posts events over XMPP)
- zabbix	(Sends events to a Zabbix server)


## 모니터링 방안 및 인터페이스

#### 모니터링 UI
-  X-Pack 사용시 UI제공 (kibana)
- Metricbeat collection
Metricbeat collects monitoring data from your Logstash instance and sends it directly to your monitoring cluster.
- Legacy collection
Legacy collectors send monitoring data to your production cluster.

#### API 제공
- Node Info API
- Plugins Info API
- Node Stats API
- Hot Threads API
