#Use a base image with Ubuntu
FROM ubuntu:latest

RUN apt-get update && \
    apt-get install -y openjdk-11-jdk && \
    apt-get install -y wget

RUN mkdir /prometheus && \
    cd /prometheus 

COPY jmx_prometheus_javaagent-0.3.0.jar /prometheus/jmx_prometheus_javaagent-0.3.0.jar
COPY zookeeper.yml /prometheus/zookeeper.yml

# Install Zookeeper
COPY apache-zookeeper-3.9.0-bin.tar.gz /home/apache-zookeeper-3.9.0-bin.tar.gz
RUN cd /home/ && \
    tar -zxf apache-zookeeper-3.9.0-bin.tar.gz && \
    mv apache-zookeeper-3.9.0-bin /usr/local/zookeeper && \
    mkdir -p /var/lib/zookeeper && \
    export JAVA_HOME=/usr/lib/jvm/java-11-openjdk-amd64

COPY kafka_2.13-3.5.1.tgz /home/kafka_2.13-3.5.1.tgz
RUN cd /home/ && \
    tar -zxf /home/kafka_2.13-3.5.1.tgz && \
    mv kafka_2.13-3.5.1 /usr/local/kafka && \
    mkdir /tmp/kafka-logs

ENV KAFKA_OPTS="-javaagent:/prometheus/jmx_prometheus_javaagent-0.3.0.jar=8081:/prometheus/zookeeper.yml"
COPY zookeeper.properties /usr/local/kafka/config/zookeeper.properties

CMD ["/usr/local/kafka/bin/zookeeper-server-start.sh", "/usr/local/kafka/config/zookeeper.properties"]
