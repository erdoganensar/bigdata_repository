FROM ensaradaletai/bigdata_spark:3.3.2

USER root

RUN apt-get install -y python3 python3-pip python-is-python3
RUN apt-get install -y vim nano curl
RUN apt-get install -y telnet net-tools iputils-ping


# get sources
RUN apt-get install -y libpostgresql-jdbc-java
#RUN wget https://archive.apache.org/dist/hive/hive-3.1.2/apache-hive-3.1.2-bin.tar.gz -P /home/hadoop/
#RUN tar -xzf /home/hadoop/apache-hive-3.1.2-bin.tar.gz -C /home/hadoop/
COPY ./apache-hive-3.1.2-bin /home/hadoop/hive
#RUN mv /home/hadoop/apache-hive-3.1.2-bin /home/hadoop/hive
#RUN rm -rf /home/hadoop/apache-hive-3.1.2*

# set environment variables
ENV HIVE_HOME /home/hadoop/hive
#ENV SPARK_HOME /home/hadoop/spark
ENV HIVE_CONF_DIR=$HIVE_HOME/conf
#ENV SPARK_CONF_DIR=$SPARK_HOME/conf
ENV PATH=$HIVE_HOME/sbin:$HIVE_HOME/bin:$PATH

COPY configs/postgresql-42.7.0.jar /home/hadoop/hive/lib/
ADD configs/hive-site.xml $HIVE_CONF_DIR/hive-site.xml
RUN echo "export HADOOP_HOME=/home/hadoop/hadoop" >> /home/hadoop/hive/bin/hive-config.sh
RUN echo "export SPARK_HOME=/home/hadoop/spark" >> /home/hadoop/hive/bin/hive-config.sh

#RUN rm /home/hadoop/hive/lib/guava-19.0.jar
#RUN cp /home/hadoop/hadoop/share/hadoop/hdfs/lib/guava-27.0-jre.jar /home/hadoop/hive/lib/

#RUN ln $HIVE_CONF_DIR/hive-site.xml $SPARK_CONF_DIR/

RUN chown hadoop -R /home/hadoop/hive
RUN export LANGUAGE=en_US.UTF-8
