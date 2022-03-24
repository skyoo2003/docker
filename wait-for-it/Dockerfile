FROM alpine:3.15.2
LABEL maintainer="Sung-Kyu Yoo <skyoo2003@gmail.com>"

ADD https://raw.githubusercontent.com/vishnubob/wait-for-it/master/wait-for-it.sh /usr/bin/wait-for-it.sh

RUN set -e && \
    apk update && \
    apk add --no-cache --virtual .exec-deps bash && \
    chmod +x /usr/bin/wait-for-it.sh

ENTRYPOINT ["wait-for-it.sh"]
