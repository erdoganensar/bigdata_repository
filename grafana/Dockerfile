FROM ubuntu:22.04

ENV INFLUXDB_VERSION 1.8.10
ENV GRAFANA_VERSION 10.2.3
ENV ARCH amd64

RUN apt-get update
RUN apt-get install -qq -y curl libfontconfig musl
COPY configs/grafana-enterprise_10.2.3_amd64.deb /tmp
RUN dpkg -i tmp/grafana-enterprise_10.2.3_amd64.deb
COPY configs/influxdb_1.8.10_amd64.deb /tmp
RUN dpkg -i tmp/influxdb_1.8.10_amd64.deb


COPY configs/influxdb.conf /etc/influxdb/influxdb.conf
COPY --chown=grafana:grafana configs/grafana_dashboards /var/lib/grafana/dashboards
COPY --chown=grafana:grafana configs/influx.yaml /etc/grafana/provisioning/datasources/influx.yaml
COPY --chown=grafana:grafana configs/spark.yaml /etc/grafana/provisioning/dashboards/spark.yaml
COPY grafana/entrypoint.sh /opt/entrypoint.sh

# expose grafana dashboard (3000) and influxdb graphite (2003) and http (8086) endpoints
EXPOSE 3000/tcp 2003/tcp 8086/tcp

RUN chmod 777 /opt/entrypoint.sh
WORKDIR /
ENTRYPOINT [ "/opt/entrypoint.sh" ]
