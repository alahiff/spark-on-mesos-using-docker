FROM ubuntu:16.04

# Dependencies for Spark
# https://github.com/apache/spark/blob/master/external/docker/spark-mesos/Dockerfile
RUN apt-get update && \
    apt-get install -y python libnss3 openjdk-8-jre-headless curl scala && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

# Adds Mesosphere official repository, and install Mesos
# It is needed for libmesos.so
# https://open.mesosphere.com/getting-started/install/
RUN apt-get -y update && \
    apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv E56151BF && \
    echo "deb http://repos.mesosphere.com/ubuntu xenial main" | \
    tee /etc/apt/sources.list.d/mesosphere.list && \
    apt-get -y update && \
    apt-get -y install mesos && \
    apt-get clean && rm -rf /var/lib/apt/lists/*

# Install Spark
RUN mkdir /opt/spark && \
    curl http://d3kbcqa49mib13.cloudfront.net/spark-2.1.0-bin-hadoop2.7.tgz | \
    tar --strip-components=1 -xzC /opt/spark

ENV SCALA_HOME /usr/local/share/scala
ENV PATH $PATH:$SCALA_HOME/bin
ENV SPARK_HOME /opt/spark
ENV PATH $PATH:/opt/spark/bin
WORKDIR /opt/spark
