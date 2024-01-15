FROM postgres

USER root

RUN apt-get update && \
    apt-get install -y nano telnet curl net-tools vim 

RUN export LANGUAGE=en_US.UTF-8
COPY configs/init.sh	/docker-entrypoint-initdb.d/init.sh
