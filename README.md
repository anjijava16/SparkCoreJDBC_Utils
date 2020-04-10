# SparkCoreJDBC_Utils

## Spark Read/ Write JDBC parameters :
    // Get the Connection
    val JDBC_URL = newOption("url")
    val JDBC_TABLE_NAME = newOption("dbtable")
    val JDBC_QUERY_STRING = newOption("query")
    val JDBC_DRIVER_CLASS = newOption("driver")
    val JDBC_PARTITION_COLUMN = newOption("partitionColumn")

    // Read Only 
    val JDBC_LOWER_BOUND = newOption("lowerBound")
    val JDBC_UPPER_BOUND = newOption("upperBound")

    // It will use read and write als 
    val JDBC_NUM_PARTITIONS = newOption("numPartitions")
    val JDBC_QUERY_TIMEOUT = newOption("queryTimeout")
    val JDBC_BATCH_FETCH_SIZE = newOption("fetchsize")
    val JDBC_TRUNCATE = newOption("truncate")
    val JDBC_CASCADE_TRUNCATE = newOption("cascadeTruncate")
    val JDBC_CREATE_TABLE_OPTIONS = newOption("createTableOptions")
    val JDBC_CREATE_TABLE_COLUMN_TYPES = newOption("createTableColumnTypes")
    val JDBC_CUSTOM_DATAFRAME_COLUMN_TYPES = newOption("customSchema")

    // Write Only 
    val JDBC_BATCH_INSERT_SIZE = newOption("batchsize")
    val JDBC_TXN_ISOLATION_LEVEL = newOption("isolationLevel")
    val JDBC_SESSION_INIT_STATEMENT = newOption("sessionInitStatement")
    val JDBC_PUSHDOWN_PREDICATE = newOption("pushDownPredicate")
    val JDBC_KEYTAB = newOption("keytab")
    val JDBC_PRINCIPAL = newOption("principal")



## Read the JDBC :
    Dataset<Row> set = spark.read("jdbc")
      .option("url", url)
      .option("dbtable", "pets")
      .option("user", user)
      .option("password", password)
      .option("numPartitions", 10)
      .option("partitionColumn", "owner_id")
      .option("lowerBound", 1)
      .option("upperBound", 10000)
      .load()

    partitionColumn — numeric column name of a table in question
    lowerBound — minimal value to read
    upperBound— maximal value to read	
# This will result into parallel queries like:
    SELECT * FROM pets WHERE owner_id >= 1 and owner_id < 1000
    SELECT * FROM pets WHERE owner_id >= 1000 and owner_id < 2000
    SELECT * FROM pets WHERE owner_id >= 2000 and owner_id < 3000



## Write the JDBC 
      df.write.format("jdbc")
        .option("dbtable", "TEST.SAVETEST")
        .option("url", url1)
        .option("user", "testUser")
        .option("password", "testPass")
        .option("numPartitions", "5")
        .option("batchsize", "100000")
        .save()

# Writing Modes:
   # SaveMode.Overwrite
    Rewrite the whole table (SaveMode.Overwrite). According to the database driver, some databases are treated with TRUNCATE TABLE command, others are dropped and completely recreated with indices and foreign keys lost. See https://issues.apache.org/jira/browse/SPARK-16463

   # SaveMode.Append 
    Append data to existing without conflicting with primary keys / indexes (SaveMode.Append)
   # SaveMode.Ignore
    Ignore any conflict (even existing table) and skip writing (SaveMode.Ignore)
   # SaveMode.ErrorIfExists
    Create a table with data or throw an error when exists (SaveMode.ErrorIfExists)


# Spark Source Code:
https://github.com/apache/spark/blob/master/sql/core/src/main/scala/org/apache/spark/sql/execution/datasources/jdbc/JdbcUtils.scala
https://github.com/apache/spark/blob/master/sql/core/src/main/scala/org/apache/spark/sql/execution/datasources/jdbc/JDBCOptions.scala

https://github.com/apache/spark/blob/master/sql/core/src/test/scala/org/apache/spark/sql/jdbc/JDBCWriteSuite.scala

http://www.gatorsmile.io/numpartitionsinjdbc/



 
