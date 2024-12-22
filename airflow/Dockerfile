FROM apache/airflow:latest-python3.12

# Права администратора
USER root

# Обновление и установка пакетных менеджеров
RUN apt-get update && \
    apt-get install -y apt-utils && \
    apt-get install -y wget

# Установка JDK
RUN wget https://download.oracle.com/java/22/latest/jdk-22_linux-x64_bin.tar.gz && \
    mkdir -p /opt/java && \
    tar -xvf jdk-22_linux-x64_bin.tar.gz -C /opt/java && \
    rm jdk-22_linux-x64_bin.tar.gz

# Установка JAVA
RUN wget -O jre-8u421-linux-x64.tar.gz https://javadl.oracle.com/webapps/download/AutoDL?BundleId=250118_d8aa705069af427f9b83e66b34f5e380 && \
    tar -xvf jre-8u421-linux-x64.tar.gz -C /opt/java && \
    rm jre-8u421-linux-x64.tar.gz

# Установка Apache Spark
RUN wget https://downloads.apache.org/spark/spark-3.5.3/spark-3.5.3-bin-hadoop3.tgz && \
    mkdir -p /opt/spark && \
    tar -xvf spark-3.5.3-bin-hadoop3.tgz -C /opt/spark && \
    rm spark-3.5.3-bin-hadoop3.tgz

# загрузка JDBC-драйверов для PostgreSQL и ClickHouse и перенос их в папку jars
RUN wget https://jdbc.postgresql.org/download/postgresql-42.7.4.jar && \
    wget https://github.com/ClickHouse/clickhouse-java/releases/download/v0.6.4/clickhouse-jdbc-0.6.4-all.jar && \
    mv postgresql-42.7.4.jar /opt/spark/spark-3.5.3-bin-hadoop3/jars && \
    mv clickhouse-jdbc-0.6.4-all.jar /opt/spark/spark-3.5.3-bin-hadoop3/jars

# Возвращение к пользователю по умолчанию
USER airflow

# Установка переменных окружения
ENV JAVA_HOME=/opt/java/jdk-22.0.2
ENV SPARK_HOME=/opt/spark/spark-3.5.3-bin-hadoop3
ENV PATH=$PATH:$JAVA_HOME/bin
ENV PATH=$PATH:$SPARK_HOME/bin
ENV PYTHONPATH=$SPARK_HOME/python/lib/py4j-0.10.9.7-src.zip

# Установка остальных пакетов через pip
COPY requirements.txt /requirements.txt
RUN pip install --upgrade pip
RUN pip install --no-cache-dir -r /requirements.txt