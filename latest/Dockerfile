# https://github.com/linuxserver/docker-baseimage-kasmvnc
FROM ghcr.io/linuxserver/baseimage-kasmvnc:ubuntunoble

# Install required dependencies.
RUN set -x \
 && apt-get update \
 && apt-get install -y --no-install-recommends \
        default-jre \
        libopencv-highgui406t64 \
        libtesseract5 \
        libxft2 \
        wmctrl \
        xdotool \
 && apt-get clean \
 && rm -rf /var/lib/apt/lists/* /tmp/*

# Download SikuliX.
# http://www.sikulix.com/
# https://raiman.github.io/SikuliX1/downloads.html
# $ curl -L https://launchpad.net/sikuli/sikulix/ | grep -Po '(?<=href=")https://launchpad.net/sikuli/[^"]+?sikulixide-[0-9.]+\-lux.jar' | head -n1
ARG SIKULIX_DOWNLOAD_URL="https://launchpad.net/sikuli/sikulix/2.0.5/+download/sikulixide-2.0.5-lux.jar"

RUN mkdir /opt/sikulix \
 && curl -L -o /opt/sikulix/sikulix.jar $SIKULIX_DOWNLOAD_URL

ENV TITLE=SikuliX

# Autostart and menus.
COPY /root /

# Optional applications.
# Firefox, fork of https://github.com/linuxserver/docker-firefox/blob/master/Dockerfile
RUN set -x \
 && apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 738BEB9321D1AAEC13EA9391AEBDF4819BE21867 \
 && echo "deb https://ppa.launchpadcontent.net/mozillateam/ppa/ubuntu noble main" > /etc/apt/sources.list.d/firefox.list \
 && apt-get update \
 && apt-get install -y --no-install-recommends \
        firefox \
        ^firefox-locale \
 && apt-get clean \
 && rm -rf /var/lib/apt/lists/* /tmp/*

# scrcpy to remote control an Android device.
RUN set -x \
 && apt-get update \
 && apt-get install -y --no-install-recommends \
        adb \
        scrcpy \
 && apt-get clean \
 && rm -rf /var/lib/apt/lists/* /tmp/*

# When using SikuliX we often swap betwen apps.
# It's easier without auto-fullscreen.
ENV NO_FULL=true

EXPOSE 3000
