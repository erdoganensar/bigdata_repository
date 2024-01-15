FROM ensaradaletai/bigdata_hadoop:3.3.1

USER root

RUN apt-get install -y python3 python3-pip python-is-python3
RUN apt-get install -y vim nano curl
RUN apt-get install -y telnet net-tools iputils-ping

# get sources
RUN mkdir /usr/share/scala
#RUN wget https://downloads.lightbend.com/scala/2.13.12/scala-2.13.12.tgz -P /tmp/
#RUN tar -xzf /tmp/scala-2.13.12.tgz -C /tmp/
#RUN mv /tmp/scala-2.13.12/* /usr/share/scala/
#COPY ./scala-2.13.12/ /usr/share/scala/
#RUN rm -rf /tmp/scala-2.13.12 /tmp/scala-2.13.12.tgz
#COPY ./scala-2.13.12/bin/* /usr/bin/



#RUN wget https://archive.apache.org/dist/spark/spark-3.3.2/spark-3.3.2-bin-hadoop3.tgz -P /home/hadoop/
#RUN tar -xzf /home/hadoop/spark-3.3.2-bin-hadoop3.tgz -C /home/hadoop/
#RUN mv /home/hadoop/spark-3.3.2-bin-hadoop3 /home/hadoop/spark
#RUN mkdir -p /home/hadoop/spark
COPY ./spark-3.3.2-bin-hadoop3 /home/hadoop/spark
#RUN rm /home/hadoop/spark-2.4.0-bin-without-hadoop.tgz

ADD configs/postgresql-42.7.0.jar /home/hadoop/spark/jars/postgresql-42.7.0.jar
#ADD configs/spark-hive_2.13-3.3.1.jar /home/hadoop/spark/jars/spark-hive_2.13-3.3.1.jar
#ADD configs/spark-hive-thriftserver_2.13-3.3.1.jar /home/hadoop/spark/jars/spark-hive-thriftserver_2.13-3.3.1.jar


RUN mkdir -p /home/hadoop/spark/logs
RUN chown hadoop -R /home/hadoop/spark/logs

# set environment variables
#ENV SCALA_HOME /usr/share/scala
ENV SPARK_HOME /home/hadoop/spark
ENV SPARK_LOG_DIR /home/hadoop/spark/logs
ENV PYTHONHASHSEED=1
ENV PYSPARK_PYTHON=python3
ENV SPARK_CONF_DIR=$SPARK_HOME/conf
# ENV SPARK_DIST_CLASSPATH $(hadoop classpath) does not work
RUN export SPARK_DIST_CLASSPATH=$(hadoop classpath)
ENV PATH $SPARK_HOME/bin:$SPARK_HOME/sbin:$PATH
RUN mv /home/hadoop/spark/conf/spark-env.sh.template /home/hadoop/spark/conf/spark-env.sh
RUN echo "export SPARK_DIST_CLASSPATH=$(hadoop classpath)" >> /home/hadoop/spark/conf/spark-env.sh
RUN echo "export SPARK_LOG_DIR=/home/hadoop/spark/logs" >> /home/hadoop/spark/conf/spark-env.sh
RUN mv /home/hadoop/spark/conf/spark-defaults.conf.template /home/hadoop/spark/conf/spark-defaults.conf
RUN echo "spark.eventLog.dir file:///home/hadoop/spark/logs" >> /home/hadoop/spark/conf/spark-defaults.conf
RUN echo "spark.history.fs.logDirectory file:///home/hadoop/spark/logs" >> /home/hadoop/spark/conf/spark-defaults.conf
#RUN echo "spark.yarn.jars " >> /home/hadoop/spark/conf/spark-defaults.conf
ADD configs/workers /home/hadoop/spark/conf/slaves

ADD configs/hive-site.xml /home/hadoop/spark/conf/
ADD configs/core-site.xml /home/hadoop/spark/conf/ 
ADD configs/hdfs-site.xml /home/hadoop/spark/conf/ 


#iceberg install and conf

#COPY requirements.txt .
#RUN pip3 install -r requirements.txt

# Add scala kernel via spylon-kernel
#RUN python3 -m spylon_kernel install

#RUN curl https://github.com/SpencerPark/IJava/releases/download/v1.3.0/ijava-1.3.0.zip -Lo ijava-1.3.0.zip \
#  && unzip ijava-1.3.0.zip \
#  && python3 install.py --sys-prefix \
#COPY configs/java /home/hadoop
#COPY configs/install.py /home/hadoop
#RUN python3 /home/hadoop/install.py --sys-prefix
#  && rm ijava-1.3.0.zip


ENV PYTHONPATH=$SPARK_HOME/python:$SPARK_HOME/python/lib/py4j-0.10.9.7-src.zip:$PYTHONPATH

# Download iceberg spark runtime
#RUN curl https://repo1.maven.org/maven2/org/apache/iceberg/iceberg-spark-runtime-${SPARK_MAJOR_VERSION}_2.12/${ICEBERG_VERSION}/iceberg-spark-runtime-${SPARK_MAJOR_VERSION}_2.12-${ICEBERG_VERSION}.jar -Lo /opt/spark/jars/iceberg-spark-runtime-${SPARK_MAJOR_VERSION}_2.12-${ICEBERG_VERSION}.jar
COPY configs/iceberg-spark-3.3_2.12-1.4.2.jar /home/hadoop/spark/jars/.

# Add iceberg spark runtime jar to IJava classpath
ENV IJAVA_CLASSPATH=/home/hadoop/spark/jars/*

RUN mkdir -p /home/hadoop/iceberg/data 
COPY configs/tg4x-b46p.json /home/hadoop/iceberg/data/nyc_film_permits.json

RUN mkdir -p /home/hadoop/iceberg/localwarehouse /home/hadoop/iceberg/notebooks /home/hadoop/iceberg/warehouse /home/hadoop/iceberg/spark-events /home/hadoop/iceberg



RUN chown hadoop -R /home/hadoop/spark
RUN chown hadoop -R /home/hadoop/iceberg
