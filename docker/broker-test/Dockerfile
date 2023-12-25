#Use a base image with Ubuntu
FROM ubuntu:latest

RUN apt-get update && \
    apt-get install -y openjdk-11-jdk && \
    apt-get install -y wget


RUN mkdir /prometheus && \
    cd /prometheus 

COPY kafka-0-8-2.yml /prometheus/kafka-0-8-2.yml
COPY jmx_prometheus_javaagent-0.3.0.jar /prometheus/jmx_prometheus_javaagent-0.3.0.jar

COPY kafka_2.13-3.5.1.tgz /home/kafka_2.13-3.5.1.tgz
RUN cd /home/ && \
    tar -zxf /home/kafka_2.13-3.5.1.tgz && \
    mv kafka_2.13-3.5.1 /usr/local/kafka && \
    mkdir /tmp/kafka-logs && \
    rm /usr/local/kafka/config/server.properties
EXPOSE 8084
COPY server.properties /usr/local/kafka/config/server.properties
ENV KAFKA_OPTS="-javaagent:/prometheus/jmx_prometheus_javaagent-0.3.0.jar=8084:/prometheus/kafka-0-8-2.yml"
CMD ["/usr/local/kafka/bin/kafka-server-start.sh", "/usr/local/kafka/config/server.properties"]
 