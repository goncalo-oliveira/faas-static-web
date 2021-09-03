FROM --platform=${TARGETPLATFORM:-linux/amd64} ghcr.io/openfaas/of-watchdog:0.8.4 AS watchdog
FROM --platform=${TARGETPLATFORM:-linux/amd64} alpine:3.14 AS build

ARG TARGETPLATFORM

COPY --from=watchdog /fwatchdog /usr/bin/fwatchdog
RUN chmod +x /usr/bin/fwatchdog

RUN apk --no-cache add curl ca-certificates \
    && addgroup -S app && adduser -S -g app app

RUN chmod 777 /tmp

USER app

WORKDIR /home/app
COPY ./function .

# TODO: build project and generate static files
#RUN ....npm or whatever...
RUN mkdir public && cp index.html public/
#

FROM alpine:3.14 AS runtime

# Set correct permissions to use non root user
WORKDIR /home/app

COPY --from=build /home/app/public public
COPY --from=build /usr/bin/fwatchdog .

ENV mode="static"
ENV static_path="/home/app/public"

ENV exec_timeout="10s"
ENV write_timeout="15s"
ENV read_timeout="15s"

ENV prefix_logs="false"

HEALTHCHECK --interval=3s CMD [ -e /tmp/.lock ] || exit 1

CMD ["./fwatchdog"]
