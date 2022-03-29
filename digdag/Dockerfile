FROM alpine:3.15.3
LABEL maintainer="Sung-Kyu Yoo <skyoo2003@gmail.com>"

ENV DIGDAG_VERSION 0.10.0

USER 0

RUN set -e && \
    addgroup --gid 1000 digdag && \
    adduser --uid 1000 --ingroup digdag --home /digdag --disabled-password digdag && \
    mkdir -p /digdag/config /digdag/bin && \
    chown -R 1000:1000 /digdag

COPY digdag.properties /digdag/config/digdag.properties
COPY pip-requirements.txt /digdag/pip-requirements.txt
RUN set -e && \
    apk update && \
    apk add --no-cache --virtual .build-deps build-base gcc g++ glib-dev openssl-dev make && \
    apk add --no-cache --virtual .exec-deps bash rsync curl git python3 python3-dev py3-pip openssh openjdk8-jre && \
    pip3 install --no-cache-dir -r /digdag/pip-requirements.txt && \
    rm -f /digdag/pip-requirements.txt && \
    curl -o /digdag/bin/digdag.jar --create-dirs -L "https://dl.digdag.io/digdag-${DIGDAG_VERSION}" && \
    chmod +x /digdag/bin/digdag.jar && \
    apk del .build-deps

EXPOSE 65432
WORKDIR /digdag
USER 1000:1000

ENTRYPOINT [ "java", "-jar", "/digdag/bin/digdag.jar" ]
CMD [ "server", "--config", "/digdag/config/digdag.properties" ]
