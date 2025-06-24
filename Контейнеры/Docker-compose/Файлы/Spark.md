```docker
version: '3'  
services:  
spark-master:  
image: bitnami/spark:3.5.0  
ports:  
- "8080:8080" # Web UI for the master node  
- "7077:7077" # Master port  
environment:  
SPARK_MODE: master  
SPARK_MASTER_HOST: spark-master  
SPARK_MASTER_PORT: 7077  
SPARK_MASTER_WEBUI_PORT: 8080  
SPARK_WORKER_CORES: 2  
SPARK_WORKER_MEMORY: 2G  
SPARK_EXECUTOR_CORES: 2  
SPARK_EXECUTOR_MEMORY: 2G  
volumes:  
- ./notebooks:/opt/bitnami/spark/work-dir  
  
spark-worker:  
image: bitnami/spark:3.5.0  
depends_on:  
- spark-master  
environment:  
SPARK_MODE: worker  
SPARK_MASTER_URL: spark://spark-master:7077  
SPARK_WORKER_CORES: 2  
SPARK_WORKER_MEMORY: 2G  
SPARK_EXECUTOR_CORES: 2  
SPARK_EXECUTOR_MEMORY: 2G  
ports:  
- "8081:8081" # Worker web UI  
volumes:  
- ./notebooks:/opt/bitnami/spark/work-dir  
  
jupyter-notebook:  
image: jupyter/pyspark-notebook:python-3.11  
ports:  
- "8888:8888" # Jupyter notebook interface  
- "4040:4040"  
environment:  
PYSPARK_DRIVER_PYTHON: ipython  
PYSPARK_SUBMIT_ARGS: "--master spark://spark-master:7077 pyspark-shell"  
volumes:  
- ./notebooks:/home/jovyan/work```
```
При работе с PySpark нужно устанавливать одинаковую версию для spark-master, spark-worker и в окружении Python, иначе могут возникать ошибки при работе

Пример Python кода

```python
#pip install pyspark==3.5.0

from pyspark.sql import SparkSession


spark = SparkSession.builder.appName("Egor").config("spark.master", "spark://spark-master:7077").getOrCreate()

data = [(1, "Apple"), (2, "Banana"), (3, "Cherry")]
columns = ["id", "fruit"]
df = spark.createDataFrame(data, columns)

df.show()
```

### Добавил пару строк для работы с Postgres

```docker
version: '3'

  
services:

  spark-master:

    image: bitnami/spark:3.5.0

    ports:

      - "8080:8080"

      - "7077:7077"

    environment:

      SPARK_MODE: master

      SPARK_MASTER_HOST: spark-master

      SPARK_MASTER_PORT: 7077

      SPARK_MASTER_WEBUI_PORT: 8080

      SPARK_WORKER_CORES: 2

      SPARK_WORKER_MEMORY: 2G

      SPARK_EXECUTOR_CORES: 2

      SPARK_EXECUTOR_MEMORY: 2G

      # Передаем опцию для загрузки JDBC драйвера

      SPARK_SUBMIT_OPTIONS: "--jars /opt/bitnami/spark/jars/postgresql-42.5.6.jar"

    volumes:

      - ./notebooks:/opt/bitnami/spark/work-dir

      - ./drivers/postgresql-42.5.6.jar:/opt/bitnami/spark/jars/postgresql-42.5.6.jar

  

  spark-worker:

    image: bitnami/spark:3.5.0

    depends_on:

      - spark-master

    environment:

      SPARK_MODE: worker

      SPARK_MASTER_URL: spark://spark-master:7077

      SPARK_WORKER_CORES: 2

      SPARK_WORKER_MEMORY: 2G

      SPARK_EXECUTOR_CORES: 2

      SPARK_EXECUTOR_MEMORY: 2G

      SPARK_SUBMIT_OPTIONS: "--jars /opt/bitnami/spark/jars/postgresql-42.5.6.jar"

    ports:

      - "8081:8081"

    volumes:

      - ./notebooks:/opt/bitnami/spark/work-dir

      - ./drivers/postgresql-42.5.6.jar:/opt/bitnami/spark/jars/postgresql-42.5.6.jar

  

  jupyter-notebook:

    image: jupyter/pyspark-notebook:python-3.11

    ports:

      - "8888:8888"

      - "4040:4040"

    environment:

      PYSPARK_DRIVER_PYTHON: ipython

      PYSPARK_SUBMIT_ARGS: "--master spark://spark-master:7077 --jars /opt/bitnami/spark/jars/postgresql-42.5.6.jar pyspark-shell"

    volumes:

      - ./notebooks:/home/jovyan/work

      - ./drivers/postgresql-42.5.6.jar:/opt/bitnami/spark/jars/postgresql-42.5.6.jar
```

Нужно в папку с проектом создать папку drivers и положить туда драйвер