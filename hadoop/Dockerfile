FROM ubuntu:latest

USER root

# open port 22
EXPOSE 22

RUN apt-get update

# OpenJDK installation
RUN apt-get install -y openjdk-8-jre openjdk-8-jdk 
RUN apt-get install -y openssh-server 
RUN apt-get install -y vim nano curl
RUN apt-get install -y python3 python3-pip python-is-python3
RUN apt-get install -y telnet net-tools iputils-ping
 
# add hadoop user
RUN useradd -m -s /bin/bash hadoop

# set pubkey authentication
RUN echo "PubkeyAuthentication yes" >> /etc/ssh/ssh_config
RUN mkdir -p /home/hadoop/.ssh
RUN echo "PubkeyAcceptedKeyTypes +ssh-dss" >> /home/hadoop/.ssh/config
RUN echo "PasswordAuthentication no" >> /home/hadoop/.ssh/config

# copy keys
ADD configs/id_rsa.pub /home/hadoop/.ssh/id_rsa.pub
ADD configs/id_rsa /home/hadoop/.ssh/id_rsa
RUN chmod 400 /home/hadoop/.ssh/id_rsa
RUN chmod 400 /home/hadoop/.ssh/id_rsa.pub
RUN cat /home/hadoop/.ssh/id_rsa.pub >> /home/hadoop/.ssh/authorized_keys
RUN chown hadoop -R /home/hadoop/.ssh


CMD service ssh start && bash
