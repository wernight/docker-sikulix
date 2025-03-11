FROM java:8

RUN echo "Install requirements:" \
 && apt-get update \
 && apt-get install -y --no-install-recommends \
        libopencv-core2.4 \
        libopencv-highgui2.4 \
        libtesseract3 \
        wmctrl \
        xdotool \
        xvfb \
        xauth \
 && apt-get clean \
 && rm -rf /var/lib/apt/lists/* /tmp/*

RUN echo "Workaround lsb_release showing errors when missing:" \
 && printf "#/bin/sh\nuname -a\n" >/usr/bin/lsb_release \
 && chmod +x /usr/bin/lsb_release

# Note: Using latest **SikuliX with Jypthon** nightly build 1.1.1 because it's the first that can still run even when *wmctrl* isn't fully working, on Xvfb.
RUN echo "Download and install SikuliX:" \
 && DOWNLOAD_URL=$(curl -L http://nightly.sikuli.de/ | grep -Po '(?<=href=")https://[^"]+/sikulixsetup-[^"]+\.jar') \
 && mkdir /sikulix \
 && curl -L $DOWNLOAD_URL -o /sikulix/sikulix-setup.jar \
 && xvfb-run java -jar /sikulix/sikulix-setup.jar options 1.1 \
\
 && echo "Clean-up:" \
 && apt-get purge --auto-remove -y \
        xvfb \
 && rm -rf /var/lib/apt/lists/* /tmp/* /sikulix/sikulix-setup.jar /sikulix/*-SetupLog.txt /sikulix/SetupStuff

RUN echo "Make SikuliX binaries available for everyone:" \
 && chmod +x /sikulix/sikulix.jar /sikulix/runsikulix \
 && ln -s /sikulix/runsikulix /usr/local/bin/ \
 && ln -s /sikulix/sikulix.jar /usr/local/bin/

RUN echo "Create default home directory:" \
 && mkdir /home/sikulix \
 && chmod ugo+rwx /home/sikulix

VOLUME /home/sikulix

ENV HOME /home/sikulix
ENV _JAVA_OPTIONS -Duser.home=/home/sikulix

CMD ["/sikulix/runsikulix"]
