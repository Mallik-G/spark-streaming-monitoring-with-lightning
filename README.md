# spark-streaming-monitoring-with-lightning
- Real-time Spark Stream Monitoring over SocketStream.
- Using Apache Spark and Lightning Graph server. 

## 1. Background:
ApacheSpark 2.x streaming application with Dataset’s is not supporting ```streaming``` tab now. This project shows how to have a realtime graph monitoring system using Lightning-viz where we can plot and monitor any custom param that we need.

There are 3 components in this project:
1. **SparkApplication:** Spark application receives streaming data from a socket stream and it does simple job of word count.
2. **Lightning Server:** Plots a graph of ```processing time taken per batch``` and ```number of records per batch``` in real-time which gets updated as and when a new batch of input is processed by spark application
3. **StreamingListener:** Registered a custom streaming listener to handle onBatchComplete() event where we post to LightningServer.

## 2. RunningExample
Following picture depicts side-by-side view of spark-metrics page and its corresponding **```processing time taken per batch``` and ```number of records per batch```** plotted live
![Running example](https://user-images.githubusercontent.com/22542670/27770239-5e636fa6-5f58-11e7-8b72-28470de103dd.png)

## 3. Building
This project is using [mvn](#mvn), [scala 2.11](#scala), [spark 2.x](#spark) and [java 1.8](#java).
```sh
$ mvn clean install
```

## 4. Pre-execution
### 4.1. Lightning Graph Server
First of all, the application depends on Lightning Graph Server.
The default server is http://localhost:3000. You can [![Deploy](https://www.herokucdn.com/deploy/button.svg)](https://heroku.com/deploy?template=https://github.com/lightning-viz/lightning/tree/v1.2.1) or [Install](#lightning) on your machine. Good part is installing it is very simple (kinda one-click process).

## 5. Execution
Once lightning server is up & running, We can start our spark application in either of the 2 ways listed below.
First of all, there are 2 ways to execute the application:

1. **Standalone jar**
```sh
$ scala -extdirs “$SPARK_HOME/lib" <path-to-spark-streaming-monitoring-with-lightning.jar> --master <master> <cmd-line-args>
```

2. **spark-submit**
```sh
$ spark-submit --master <master> <path-to-spark-streaming-monitoring-with-lightning.jar> <cmd-line-args>
```
Default value for master is local[2].

### 5.1 <cmd-line-args>
Optionally, you can provide configuration params like lightning server url etc from command line.
To see the list of configurable params, just type:
```sh
$ <SUBMIT-COMMAND> --help
OR
<SUBMIT-COMMAND> -h
```
It'll look something like this:
```markdown
This is a Spark Streaming application which receives data from SocketStream and does word count.
You can monitor batch size and batch processing time by real-time graph that's rendered using
Lightning graph server. So, this application needs lightningServerUrl and SocketStreamHost
and Port from where to listen to..
Usage: spark-submit realtime-spark-monitoring-with-lightning*.jar [options]

  Options:
  -h, --help
  -m, --master <master_url>                    spark://host:port, mesos://host:port, yarn, or local.
  -n, --name <name>                            A name of your application.
  -ssh, --socketStreamHost <hostname>          Default: localhost
  -ssp, --socketStreamPort <port>              Default: 9999
  -bi, --batchInterval <batch interval in ms>  Default: 5
  -ls, --lightningServerUrl <hostname>  T      Default: http://localhost:3000
```

### 5.2. File configuration
Default values for all the options available from command-line are also present in configuration file. You can directly tweak the file instead of submitting it every time from run/submit command. You can find config file at **/src/main/resources/dev/application.properties**. Once you finish tweaking the default, just do mvn build and run. Following lists the params listed in the file:
```ini
...
sparkMaster=local[2]
socketStreamPort=9999
socketStreamHost=localhost
appName=sparkmonitoring-with-lightning
batchInterval=5
lightningServerUrl=http://localhost:3000
...
```

##  6.References
- [SocketStream word count referred from spark guide](https://spark.apache.org/docs/latest/streaming-programming-guide.html)
- [Lightning Viz](http://lightning-viz.org/usage/#creating)
