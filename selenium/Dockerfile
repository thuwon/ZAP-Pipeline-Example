FROM alpine:latest

USER root
WORKDIR /usr/src/app
RUN apk update && apk add python3-dev curl py3-pip
RUN pip3 install python-owasp-zap-v2.4

RUN apk update \
  && apk add python3 \
  && apk add curl \
  && apk add chromium chromium-chromedriver \
  && apk add build-base libffi-dev openssl-dev \
  && rm -rf /var/cache/apk/*

RUN pip3 install seleniumbase
RUN pip3 install pytest

RUN seleniumbase install chromedriver

COPY ./selenium/tests/* user/src/app/

RUN mkdir -p /results
