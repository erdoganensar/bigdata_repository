FROM trinodb/trino

USER root

#RUN apt-get update && \
#    apt-get install -y nano telnet curl net-tools vim iputils-ping

#RUN apt-get update && apt-get install -y vim
RUN yum install nano -y

ADD configs/core-site.xml /etc/trino/.
ADD configs/hdfs-site.xml /etc/trino/.
ADD configs/hive-site.xml /etc/trino/.
ADD configs/hive.properties /etc/trino/catalog/.
ADD configs/iceberg.properties /etc/trino/catalog/.



EXPOSE 8080
USER trino:trino
CMD ["/usr/lib/trino/bin/run-trino"]
HEALTHCHECK --interval=10s --timeout=5s --start-period=10s \
CMD /usr/lib/trino/bin/health-check
