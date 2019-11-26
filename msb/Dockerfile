# Docker file to create an image for a hass.io add-on that contains enough software to listen to events via RTL_SDR/RTL_433 and then publish them to a MQTT broker.
# The script resides in a volume and can be modified to meet your needs.
# This hass.io addon is based on Chris Kacerguis' project here: https://github.com/chriskacerguis/honeywell2mqtt,
# which is in turn based on Marco Verleun's rtl2mqtt image here: https://github.com/roflmao/rtl2mqtt

# IMPORTANT: The container needs privileged access to /dev/bus/usb on the host.

ARG BUILD_FROM
FROM $BUILD_FROM

ENV LANG C.UTF-8

MAINTAINER Brett Clark

LABEL Description="This image is used to help setup the mqtt-bridge-smartthings docker install"

#
# First install software packages needed to compile rtl_433 and to publish MQTT events
#
RUN docker pull mbs-alpine sgupta99/mqtt-bridge-smartthings:1.0.3-alpine && \
    docker run -p:8080:8080 \
		   -v /home/pi/docker/volumes/mbs:/mbs/config \
		   -e TZ=America/New_York \
		   --name mbs-alpine \
		   sgupta99/mqtt-bridge-smartthings:1.0.3-alpine
#
# Define an environment variable
#
# Use this variable when creating a container to specify the MQTT broker host.
ENV MQTT_HOST="hassio.local"
ENV MQTT_USER="guest"
ENV MQTT_PASS="guest"
ENV MQTT_TOPIC="homeassistant/sensor/rtl433"

CMD cd /msb/config && wget -O devices.yml https://github.com/sgupta999/mqtt-bridge-smartthings/blob/master/_devices.example.yml && wget -O config.yml https://github.com/sgupta999/mqtt-bridge-smartthings/blob/master/_config.example.yml
