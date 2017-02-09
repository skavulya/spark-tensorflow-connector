# spark-tensorflow-connector

This repo contains library for loading and storing TensorFlow records with [Apache Spark](http://spark.apache.org/). The library implements data import from the standard TensorFlow record format ([TFRecords]( https://www.tensorflow.org/how_tos/reading_data/)) into Spark SQL DataFrames, and data export from DataFrames to TensorFlow records.

## What's new

This is the initial release of the `spark-tensorflow-connector` repo.

## Known issues

None.

## Prerequisites

1. [Apache Spark 2.0 (or later)](http://spark.apache.org/)

2. [Apache Maven](https://maven.apache.org/)


The following code snippet demonstrates usage.



```scala
import org.apache.commons.io.FileUtils
import org.apache.spark.sql.{ DataFrame, Row }
import org.apache.spark.sql.catalyst.expressions.GenericRow
import org.apache.spark.sql.types._
import org.scalatest.{ BeforeAndAfterAll, Matchers }
import org.tensorflow.TestingSparkSessionWordSpec

val path = s"$TF_SANDBOX_DIR/output25.tfr"
val testRows: Array[Row] = Array(
new GenericRow(Array[Any](11, 1, 23L, 10.0F, 14.0, List(1.0, 2.0), "r1")),
new GenericRow(Array[Any](21, 2, 24L, 12.0F, 15.0, List(2.0, 2.0), "r2")))
val schema = StructType(List(StructField("id", IntegerType), StructField("IntegerTypelabel", IntegerType), StructField("LongTypelabel", LongType), StructField("FloatTypelabel", FloatType), StructField("DoubleTypelabel", DoubleType), StructField("vectorlabel", ArrayType(DoubleType, true)), StructField("name", StringType)))
val rdd = sparkSession.sparkContext.parallelize(testRows)

//Save DataFrame as TFRecords
val df: DataFrame = sparkSession.createDataFrame(rdd, schema)
df.write.format("tensorflow").save(path)

//Read TFRecords into DataFrame
val importedDf: DataFrame = sparkSession.read.format("tensorflow").load(path)
importedDf.show()

```
