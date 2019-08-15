FROM "node:8.16.0-alpine"
COPY ./conf/crontab /etc/crontabs/root
RUN apk update && apk add nano
CMD ["crond", "-f", "-d", "8"]
