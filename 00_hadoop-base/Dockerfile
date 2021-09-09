FROM debian:9

ENV JAVA_HOME /usr/lib/jvm/java-8-openjdk-amd64

ENV HADOOP_HOME /opt/hadoop
ENV HIVE_HOME /opt/hive
ENV SPARK_HOME /opt/spark

ENV HADOOP_MAPRED_HOME ${HADOOP_HOME}
ENV HADOOP_COMMON_HOME ${HADOOP_HOME}
ENV HADOOP_HDFS_HOME ${HADOOP_HOME}
ENV YARN_HOME ${HADOOP_HOME}
ENV HADOOP_CONF_DIR ${HADOOP_HOME}/etc/hadoop
ENV YARN_CONF_DIR ${HADOOP_HOME}/etc/hadoop

ENV PATH $PATH:$HADOOP_HOME/bin
ENV PATH $PATH:$HADOOP_HOME/sbin
ENV PATH $PATH:$HIVE_HOME/bin
ENV PATH $PATH:$SPARK_HOME/bin

RUN apt-get update &&\
    apt-get install -y openjdk-8-jdk wget net-tools netcat procps scala python &&\
    cd /opt && wget https://archive.apache.org/dist/hadoop/core/hadoop-2.7.7/hadoop-2.7.7.tar.gz &&\
    tar xzf hadoop-2.7.7.tar.gz &&\
    rm hadoop-2.7.7.tar.gz &&\
    mv hadoop-2.7.7 hadoop

RUN cd /opt &&\ 
    wget https://archive.apache.org/dist/spark/spark-2.0.2/spark-2.0.2-bin-without-hadoop.tgz &&\
    tar xzf spark-2.0.2-bin-without-hadoop.tgz &&\
    rm spark-2.0.2-bin-without-hadoop.tgz &&\
    mv spark-2.0.2-bin-without-hadoop spark

RUN cd /opt && wget https://archive.apache.org/dist/hive/hive-2.3.9/apache-hive-2.3.9-bin.tar.gz &&\
    tar xzf apache-hive-2.3.9-bin.tar.gz &&\
    rm apache-hive-2.3.9-bin.tar.gz &&\
    mv apache-hive-2.3.9-bin hive &&\
    wget https://jdbc.postgresql.org/download/postgresql-9.4.1212.jar -O ${HIVE_HOME}/lib/postgresql-jdbc.jar &&\
    cd ${HIVE_HOME}/lib &&\
    cp ${SPARK_HOME}/jars/scala-library*.jar . &&\
    cp ${SPARK_HOME}/jars/spark-core*.jar . &&\
    cp ${SPARK_HOME}/jars/spark-network-common*.jar . &&\
    cp ${SPARK_HOME}/jars/spark-unsafe*.jar .

COPY core-site.xml ${HADOOP_HOME}/etc/hadoop
COPY hdfs-site.xml ${HADOOP_HOME}/etc/hadoop
COPY mapred-site.xml ${HADOOP_HOME}/etc/hadoop
COPY yarn-site.xml ${HADOOP_HOME}/etc/hadoop

COPY hive-site.xml ${HIVE_HOME}/conf
COPY hive-site.xml ${SPARK_HOME}/conf
COPY spark-env.sh ${SPARK_HOME}/conf

COPY wait.sh /usr/local/bin
COPY wait-namenode.sh /usr/local/bin
COPY wait-resourcemanager.sh /usr/local/bin