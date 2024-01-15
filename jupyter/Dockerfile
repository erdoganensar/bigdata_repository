FROM ensaradaletai/bigdata_hive:3.1.2
FROM tensorflow/tensorflow:latest-gpu-jupyter

RUN sed -i -e "s|http://archive.ubuntu.com|http://jp.archive.ubuntu.com|g" /etc/apt/sources.list \
 && apt-get -qq update  \
 && DEBIAN_FRONTEND=noninteractive apt-get -qq install --no-install-recommends \
      sudo \
      openjdk-8-jdk \
      curl \
      coreutils \
      libc6-dev \
      nano \
      telnet \
      net-tools \
     iputils-ping \
 && rm -rf /var/lib/apt/lists/*

ARG USERNAME=jupyter
ARG GROUPNAME=jupyter
ARG UID=1001
ARG GID=1001

RUN echo $USERNAME ALL=\(root\) NOPASSWD:ALL > /etc/sudoers.d/$USERNAME \
 && chmod 0440 /etc/sudoers.d/$USERNAME \
 && groupadd -g $GID $GROUPNAME \
 && useradd -m -s /bin/bash -u $UID -g $GID $USERNAME

USER $USERNAME

ENV JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64/

# Hadoop
COPY --from=ensaradaletai/bigdata_hive:3.1.2 --chown=$USERNAME:$GROUPNAME /home/hadoop/hadoop /home/hadoop/hadoop
ENV HADOOP_HOME=/home/hadoop/hadoop
ENV HADOOP_CONF_DIR=$HADOOP_HOME/etc/hadoop
ENV PATH=$HADOOP_HOME/sbin:$HADOOP_HOME/bin:$PATH
ENV LD_LIBRARY_PATH=$HADOOP_HOME/lib/native:$LD_LIBRARY_PATH

# Spark
COPY --from=ensaradaletai/bigdata_hive:3.1.2 --chown=$USERNAME:$GROUPNAME /home/hadoop/spark /home/hadoop/spark
ENV SPARK_HOME=/home/hadoop/spark
ENV PYTHONHASHSEED=1
ENV PYSPARK_PYTHON=python3
ENV SPARK_CONF_DIR=$SPARK_HOME/conf
ENV PATH=$SPARK_HOME/sbin:$SPARK_HOME/bin:$PATH

# Hive
COPY --from=ensaradaletai/bigdata_hive:3.1.2 --chown=$USERNAME:$GROUPNAME /home/hadoop/hive /home/hadoop/hive
ENV HIVE_HOME=/home/hadoop/hive
ENV HIVE_CONF_DIR=$HIVE_HOME/conf
ENV PATH=$HIVE_HOME/sbin:$HIVE_HOME/bin:$PATH

#iceberg
COPY --from=ensaradaletai/bigdata_hive:3.1.2 --chown=$USERNAME:$GROUPNAME /home/hadoop/iceberg /home/hadoop/iceberg
ENV ICEBERG_HOME=/home/hadoop/iceberg
ENV ICEBERG_VERSION=1.4.2
ENV PYTHONPATH=$SPARK_HOME/python:$SPARK_HOME/python/lib/py4j-0.10.9.7-src.zip:$PYTHONPATH


RUN sudo pip install -U pip &&\
    sudo pip install --no-cache-dir \
    pandas \
    numpy \
    openpyxl \
    findspark \
    tensorflow-serving-api \
    jupyter==1.0.0 \
    spylon-kernel==0.4.1 \
    pyiceberg[pyarrow,duckdb,pandas]==0.5.1 \
    jupysql==0.10.5 \
    matplotlib==3.8.2 \
    scipy==1.11.4 \
    duckdb-engine==0.9.4


# Add scala kernel via spylon-kernel
RUN sudo python3 -m spylon_kernel install

WORKDIR /home/$USERNAME
