# Spark Administration Notes


### Java version

- Spark must use Java 8. This means that if you type `java -version`, you should see "Java 1.8".

### Default directory

When Spark is doing a shuffle, it write intermediate data to disk. [By default](https://stackoverflow.com/a/32253558), this directory is `/tmp`. To change this directory, in `$SPARK_HOME/conf/spark-defaults.conf` set:

```
spark.local.dir /path/to/dir
```

### Job fails with "too many open files"

This [can happen during a shuffle](https://stackoverflow.com/a/25707630), since Spark writes intermediate data to disk.

> In general if a node in your cluster has C assigned cores and you run a job with X reducers then Spark will open C*X files in parallel and start writing.

This happened to me when I let Spark use 20 cores. I don't know how many reducers it uses by default.

To help alleviate this, in `$SPARK_HOME/conf/spark-env.sh`, I set `ulimit -n 4096`.
