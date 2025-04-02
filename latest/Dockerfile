# https://github.com/linuxserver/docker-baseimage-kasmvnc
FROM ghcr.io/linuxserver/baseimage-kasmvnc:ubuntunoble

# Install required dependencies.
RUN set -x \
 && apt-get update \
 && apt-get install -y --no-install-recommends \
        default-jre \
        libopencv-highgui406t64 \
        libtesseract5 \
        at-spi2-core \
        libxft2 \
        wmctrl \
        xdotool \
 && apt-get clean \
 && rm -rf /var/lib/apt/lists/* /tmp/*

# Download SikuliX.
# http://www.sikulix.com/
# https://github.com/RaiMan/SikuliX1
ARG SIKULIX_DOWNLOAD_URL="https://oss.sonatype.org/service/local/artifact/maven/redirect?r=snapshots&g=com.sikulix&a=sikulixidelux&v=2.0.6-SNAPSHOT&e=jar"

RUN mkdir /opt/sikulix \
 && curl -L -o /opt/sikulix/sikulix.jar "$SIKULIX_DOWNLOAD_URL"

ENV TITLE=SikuliX

# Autostart and menus.
COPY /root /

# Tessereact (OCR used by SikuliX when available).
RUN set -x \
 && apt-get update \
 && apt-get install -y \
        tesseract-ocr \
        tesseract-ocr-eng \
        tesseract-ocr-osd \
 && apt-get clean \
 && rm -rf /var/lib/apt/lists/* /tmp/* \
 && tesseract --version

# The latest Tess4J (Java wrapper for Tesseract) was
# downloaded from https://tess4j.sourceforge.net and placed under /opt/.

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

# SimpleScreenRecorder: Useful to capture the test or a demo.
RUN set -x \
 && apt-get update \
 && apt-get install -y --no-install-recommends \
        simplescreenrecorder \
 && apt-get clean \
 && rm -rf /var/lib/apt/lists/* /tmp/*

# key-mon
RUN set -x \
 && apt-get update \
 && apt-get install -y --no-install-recommends \
        pipx \
        git \
        python3-xlib \
        python3-gi \
        python3-gi-cairo \
        gir1.2-gtk-3.0 \
 && apt-get clean \
 && rm -rf /var/lib/apt/lists/* /tmp/* \
 && pipx install --system-site-packages "git+https://github.com/scottkirkwood/key-mon"

# When using SikuliX we often swap betwen apps.
# It's easier without auto-fullscreen.
ENV NO_FULL=true

EXPOSE 3000
