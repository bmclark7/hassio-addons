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

# Set workdir
#WORKDIR /opt/emqx


#RUN mkdir /config/emqx
#RUN mkdir /config/emqx/etc
#RUN mkdir /config/emqx/log

COPY rootfs /

#
# First install software packages needed to compile rtl_433 and to publish MQTT events
#
RUN apk add --no-cache alpine-sdk git bsd-compat-headers && \
    mv /etc/apk/repositories /etc/apk/repositories.bak && \
    echo "http://dl-cdn.alpinelinux.org/alpine/v3.10/main" >> /etc/apk/repositories && \
    echo "http://dl-cdn.alpinelinux.org/alpine/v3.10/community" >> /etc/apk/repositories && \
    apk update && \
    apk add --no-cache \
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
    cd /usr/bin && \
    wget https://s3.amazonaws.com/rebar3/rebar3 && \
    chmod a+x rebar3 && \
    ./rebar3 local install && \
    mv ~/.cache/rebar3 /usr/share/ && \
    export PATH=$PATH:/usr/share/rebar3/bin && \
#    mkdir -p /opt/emqx && \
#    cd /opt/emqx && \
#    cd /config/.storage && \
    mkdir /data && \
    cd /data && \
    git clone -b v4.2.1 https://github.com/emqx/emqx-rel.git && \
#    cd /config/.storage/emqx-rel && \
#    cd /opt/emqx/emqx-rel && \
    cd /data/emqx-rel && \
    make && \
    cd / && \
    mv data data.bak


#VOLUME "/opt/emqx"
#RUN mkdir /config/.storage/emqx
#RUN mkdir /config/.storage/emqx/etc
#RUN mkdir /config/.storage/emqx/log
#RUN	mkdir /config/.storage/emqx/data
#
# Define an environment variable
#
# Use this variable when creating a container to specify the MQTT broker host.
ENV EMQX_LOADED_PLUGINS="emqx_management,emqx_recon,emqx_retainer,emqx_dashboard,emqx_rule_engine,emqx_bridge_mqtt,emqx_web_hook,emqx_auth_username"
ENV EMQX_NAME="emqx"
ENV EMQX_HOST="127.0.0.1"
ENV EMQX_ALLOW_ANONYMOUS="false"
ENV EMQX_AUTH__USER__PASSWORD_HASH="plain"
ENV EMQX_AUTH__USER__1__USERNAME="mqtt"
ENV EMQX_AUTH__USER__1__PASSWORD="Password1782!"
ENV EMQX_NODE__COOKIE="emqx_dist_cookie"

CMD chmod +x /emqx.sh && /emqx.sh


