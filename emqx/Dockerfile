# Docker file to create an image for a hass.io add-on that contains enough software to listen to events via RTL_SDR/RTL_433 and then publish them to a MQTT broker.
# The script resides in a volume and can be modified to meet your needs.
# This hass.io addon is based on Chris Kacerguis' project here: https://github.com/chriskacerguis/honeywell2mqtt,
# which is in turn based on Marco Verleun's rtl2mqtt image here: https://github.com/roflmao/rtl2mqtt

# IMPORTANT: The container needs privileged access to /dev/bus/usb on the host.

ARG BUILD_FROM=emqx/emqx:latest
FROM $BUILD_FROM

ENV LANG C.UTF-8

MAINTAINER Brett Clark

LABEL Description="EMQ X Broker"

COPY rootfs /

# Set shell
SHELL ["/bin/bash", "-o", "pipefail", "-c"]

#RUN mkdir /config/emqx
#RUN mkdir /config/emqx/etc
#RUN mkdir /config/emqx/log
#RUN mkdir /config/emqx/data
#
# First install software packages needed to compile rtl_433 and to publish MQTT events
#
#RUN apk add --no-cache --virtuals alpine-sdk git \
#   bsd-compat-headers \
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
#    mkdir -p /config/emq && \
#    mkdir /tmp/src && \
#    cd /tmp/src && \
#    git clone -b v4.0.1 https://github.com/emqx/emqx-rel.git && \
#    cd /tmp/src/emqx-rel && \
#    make prefix=/config/emq && \
#    make install && \
#RUN mkdir /config/.storage/emqx
#RUN mkdir /config/.storage/emqx/etc
#RUN mkdir /config/.storage/emqx/log
#RUN	mkdir /config/.storage/emqx/data
#
# Define an environment variable
#
# Use this variable when creating a container to specify the MQTT broker host.
#ENV MQTT_HOST="hassio.local"
#ENV MQTT_USER="guest"
#ENV MQTT_PASS="guest"
#ENV MQTT_TOPIC="homeassistant/sensor/rtl433"
ENV EMQX_LOADED_PLUGINS="emqx_management,emqx_recon,emqx_retainer,emqx_dashboard,emqx_rule_engine,emqx_bridge_mqtt,emqx_web_hook,emqx_auth_username"
ENV EMQX_NAME="emqx"
ENV EMQX_HOST="127.0.0.1"
ENV EMQX_ALLOW_ANONYMOUS="false"
ENV EMQX_AUTH__USER__PASSWORD_HASH="plain"
ENV EMQX_AUTH__USER__1__USERNAME="mqtt"
ENV EMQX_AUTH__USER__1__PASSWORD="Password1782!"
#ENV PLATFORM_ETC_DIR="/config/emqx/etc"
#ENV PLATFORM_LOG_DIR="/config/emqx/log"
ENV PLATFORM_ETC_DIR="/config/.storage/emqx/etc"
ENV PLATFORM_LOG_DIR="/config/.storage/emqx/log"
ENV PLATFORM_DATA_DIR="/config/.storage/emqx/data"

ENTRYPOINT /usr/bin/docker-entrypoint.sh
#CMD cd / && cp /config/emq/emq.sh /emq.sh && chmod +x /emq.sh && /emq.sh

#VOLUME ["/opt/emqx"]
