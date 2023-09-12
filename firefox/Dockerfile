# For local x86_64 testing
# docker run -it --rm --device-cgroup-rule='c 199:* rmw' --device-cgroup-rule='c 226:* rmw' -v /run/user/63:/run/user/63 -v /dev/dri:/dev/dri -v /dev/galcore:/dev/galcore --entrypoint /bin/bash firefox
FROM debian:sid

LABEL maintainer="Raul Munoz <raul@foundries.io>"

ENV DEBIAN_FRONTEND noninteractive

ENV WAYLAND_USER="weston"
ENV XDG_RUNTIME_DIR="/run/user/63"
ENV WAYLAND_DISPLAY="wayland-0"

ENV FIO_HOME /home/weston
ENV FIO_USERID 63
ENV FIO_USERNAME weston

RUN groupadd -g 61 render && \
    if [ ! "${FIO_USERID}" = "0" ]; then \
        groupadd -g ${FIO_USERID} ${FIO_USERNAME}; \
        useradd -d ${FIO_HOME} -s /bin/bash -m ${FIO_USERNAME} -u ${FIO_USERID} -g ${FIO_USERID} -G video,render; \
    fi

RUN apt-get update -y && \
    apt-get install -y --no-install-recommends \
                       libexpat1 \
                       libpciaccess-dev \
                       libnss3 \
                       libatk1.0-0  \
                       libatk-bridge2.0-0 \
                       libjpeg62-turbo \
                       libxkbcommon0 \
                       libasound2 \
                       libflac-dev \
                       libflac12 \
                       libxslt1.1 \
                       libwayland-cursor0 \
                       libwayland-client0 \
                       libwayland-egl1 \
                       libwayland-server0 \
                       sway \
                       firefox \
                       vim \
                       ca-certificates \
                       fontconfig-config && \
    apt-get clean && apt-get autoremove -y && rm -rf /var/lib/apt/lists/* 

COPY --chown=${FIO_USERNAME} start.sh /

RUN ARCH=`uname -m` && ln -s /usr/lib/$ARCH-linux-gnu/libFLAC.so.12 /usr/lib/$ARCH-linux-gnu/libFLAC.so.8

USER ${FIO_USERNAME}
WORKDIR ${FIO_HOME}

ENTRYPOINT ["/start.sh"]