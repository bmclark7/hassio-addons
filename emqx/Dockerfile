# Docker file to create an image for a hass.io add-on for EMQX.

ARG BUILD_FROM=emqx:latest
# hadolint ignore=DL3006
FROM ${BUILD_FROM}

ENV LANG C.UTF-8

MAINTAINER Brett Clark

LABEL Description="This image is used to build and start the emq MQTT Broker and web client"

# Set workdir
WORKDIR /opt

# Set shell
SHELL ["/bin/bash", "-o", "pipefail", "-c"]

RUN mkdir -p /config/emqx/etc && \
      mkdir -p /config/emqx/log

#
# First install software packages needed to compile emqx
#
#RUN apk add --no-cache --virtual alpine-sdk git \
#    bsd-compat-headers \
#    erlang \
#    erlang-crypto \
#    erlang-public-key \
#    erlang-ssl \
#    erlang-parsetools \
#    erlang-runtime-tools \
#    erlang-xmerl \
#    erlang-tools \
#    erlang-stdlib \
#    erlang-dev \
#    erlang-eunit \
#    erlang-sasl \
#    erlang-os-mon \
#    erlang-mnesia \
#    erlang-observer \
#    erlang-kernel && \
#    mkdir /tmp/src && \
#    cd /tmp/src && \
#    git clone -b 4.0.1 https://github.com/emqx/emqx-rel.git && \
#    cd /tmp/src/emqx-rel && \
#    make && \
#    make install prefix=/usr/local && \
#    chmod +s /usr/local/bin/emq* && \

#
# Define an environment variable
#
# Use this variable when creating a container to specify the MQTT broker host.
#ENV MQTT_HOST="hassio.local"
#ENV MQTT_USER="guest"
#ENV MQTT_PASS="guest"
#ENV MQTT_TOPIC="homeassistant/sensor/rtl433"
ENV \
    EMQX_NAME="emqx"
    EMQX_HOST="127.0.0.1"
    EMQX_ALLOW_ANONYMOUS=false
    EMQX_LOADED_PLUGINS="emqx_recon,emqx_retainer,emqx_management,emqx_dashboard"
    PLATFORM_ETC_DIR="/config/emqx/etc"
    PLATFORM_LOG_DIR="/config/emqx/log"

ENTRYPOINT [ "/init" ]

