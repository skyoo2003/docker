FROM alpine:3.14.1
LABEL maintainer="Sung-Kyu Yoo <skyoo2003@gmail.com>"

ENV JMX_EXPORTER_VERSION 0.15.0

USER 0

COPY config.yml /exporter/config/config.yml
RUN set -e && \
    addgroup --gid 1000 exporter && \
    adduser --uid 1000 --ingroup exporter --home /exporter --disabled-password exporter && \
    mkdir -p /exporter/config /exporter/bin && \
    chown -R 1000:1000 /exporter

RUN set -e && \
    apk update && \
    apk add --no-cache --virtual .exec-deps bash curl openjdk8-jre && \
    curl -o /exporter/bin/jmx-prometheus-httpserver.jar --create-dirs -L "https://repo1.maven.org/maven2/io/prometheus/jmx/jmx_prometheus_httpserver/${JMX_EXPORTER_VERSION}/jmx_prometheus_httpserver-${JMX_EXPORTER_VERSION}-jar-with-dependencies.jar" && \
    chmod +x /exporter/bin/jmx-prometheus-httpserver.jar

EXPOSE 5556
WORKDIR /exporter
USER 1000:1000

ENTRYPOINT [ "java", "-jar", "/exporter/bin/jmx-prometheus-httpserver.jar" ]
CMD [ "5556", "/exporter/config/config.yml" ]
