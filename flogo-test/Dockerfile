FROM openfaas/of-watchdog:0.5.3 as watchdog

FROM alpine:3.9

RUN mkdir -p /home/app

COPY  engine-linux-amd64 /usr/bin/engine-linux-amd64

COPY --from=watchdog /fwatchdog /usr/bin/fwatchdog

RUN apk update && apk add --no-cache libc6-compat && apk add --no-cache rest
RUN chmod +x /usr/bin/fwatchdog

RUN chmod +x /usr/bin/engine-linux-amd64
ADD FlogoOpenFaaS.json /home/app/flogo.json

RUN chmod +rwx /home/app/flogo.json
# Add non root user
RUN addgroup -S app && adduser app -S -G app
RUN chown app /home/app

WORKDIR /home/app
USER app

ENV mode http
ENV upstream_url="http://127.0.0.1:9999"
# Populate example here - i.e. "cat", "sha512sum" or "node index.js"
ENV fprocess="engine-linux-amd64 -app flogo.json"

EXPOSE 9999


HEALTHCHECK --interval=3s CMD [ -e /tmp/.lock ] || exit 1

CMD ["fwatchdog"]