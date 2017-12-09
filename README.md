Spark-based StreamApprox
========================

This prototype implements the online adaptive stratified reservoir sampling ([OASRS] (https://dl.acm.org/citation.cfm?id=3135989&CFID=1011170257&CFTOKEN=36003206)) algorithm using Spark 2.0.2

### Build

Building Spark-based StreamApprox is the same as building Apache Spark.
See [scripts](https://github.com/streamapprox/flink-setup) for Spark/Flink building and installation instructions.

### Usage

This prototype supports a sampling function reservoirStratifiedSample() for Spark RDD by implementing the OASRS algorithm.
Users can use this function as a PairRDD function of Spark.


```scala
    val inputStream = KafkaUtils.createDirectStream[String, String, StringDecoder, StringDecoder](ssc, kafkaParams, topicsSet)

    //Take a sample using the OASRS algorithm
    val sampleItems = inputStream.map(_._2)
      .map(line => (line.split(",")(0), line.split(",")(1).toDouble))
      .transform(x => x.reservoirStratifiedSample(sampleSize))
      .reduceByKeyAndWindow((a: Double, b: Double) => (a + b), Seconds(10), Seconds(5))
    sampleItems.print()
```

### Support
* If you have any question please shoot me an email: do.le_quoc@tu-dresden.de
* Note that we are currently working to adapt our implementation with the new version (kafka-0-10) of Spark-Kafka connector.

### License
Published under GNU General Public License v2.0 (GPLv2), see [LICENSE](LICENSE)
