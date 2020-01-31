# Docker file to create an image for a hass.io add-on that contains enough software to listen to events via RTL_SDR/RTL_433 and then publish them to a MQTT broker.
# The script resides in a volume and can be modified to meet your needs.
# This hass.io addon is based on Chris Kacerguis' project here: https://github.com/chriskacerguis/honeywell2mqtt,
# which is in turn based on Marco Verleun's rtl2mqtt image here: https://github.com/roflmao/rtl2mqtt

# IMPORTANT: The container needs privileged access to /dev/bus/usb on the host.

ARG BUILD_FROM
FROM $BUILD_FROM

ENV LANG C.UTF-8

MAINTAINER Brett Clark

LABEL Description="EMQ X Broker"

#
# First install software packages needed to compile rtl_433 and to publish MQTT events
#
RUN apk add --no-cache --virtuals alpine-sdk git \
    bsd-compat-headers \
    erlang \
    erlang-crypto \
    erlang-public-key \
    erlang-ssl \
    erlang-parsetools \
    erlang-runtime-tools \
    erlang-xmerl \
    erlang-tools \
    erlang-stdlib \
    erlang-dev \
    erlang-eunit \
    erlang-sasl \
    erlang-os-mon \
    erlang-mnesia \
    erlang-observer \
    erlang-kernel && \
    mkdir -p /config/emq && \
    mkdir /tmp/src && \
    cd /tmp/src && \
    git clone -b v4.0.1 https://github.com/emqx/emqx-rel.git && \
    cd /tmp/src/emqx-rel && \
    make prefix=/config/emq && \
    make install && \

#
# Define an environment variable
#
# Use this variable when creating a container to specify the MQTT broker host.
ENV MQTT_HOST="hassio.local"
ENV MQTT_USER="guest"
ENV MQTT_PASS="guest"
ENV MQTT_TOPIC="homeassistant/sensor/rtl433"

CMD cd / && ./config/emq/bin/emqx start
