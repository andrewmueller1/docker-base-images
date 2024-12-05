# Base image
FROM lab/ubuntu:24.04

# Maintainer information
LABEL maintainer="andymllr <andrew.mueller1@gmail.com>"

# Disable TTY interaction
ARG DEBIAN_FRONTEND=noninteractive

# Define directories to take ownership of
ENV CHOWN_DIRS "/app,/steamcmd"

# Install dependencies which mainly fix issues with libcurl and steamcmd
RUN add-apt-repository multiverse && \
    dpkg --add-architecture i386 && \
    echo steam steam/question select "I AGREE" | sudo debconf-set-selections && \
    echo steam steam/license note '' | sudo debconf-set-selections && \
    apt-get update && \
    apt-get install -y --no-install-recommends \
        python3-full \
        python3-pip \
        steamcmd \
        lib32gcc-s1 \
        libstdc++6 \
        libsdl2-2.0-0:i386 \
        libcurl4-openssl-dev:i386 && \
    ln -sf /usr/games/steamcmd /usr/local/bin/steamcmd && \
    ls -la /usr/lib/*/libcurl.so* && \
    ln -sf /usr/lib/i386-linux-gnu/libcurl.so.4 /usr/lib/i386-linux-gnu/libcurl.so && \
    ln -sf /usr/lib/i386-linux-gnu/libcurl.so.4 /usr/lib/i386-linux-gnu/libcurl.so.3 && \
    apt-get clean && \
    rm -rf \
        /var/lib/apt/lists/* \
        /var/tmp/* \
        /tmp/dumps \
        /tmp/*

# Install botocore
RUN echo $(which pip)
# RUN pip install botocore

# Mount /steamcmd (rust-server) volume
RUN mkdir -p /steamcmd		
RUN touch /steamcmd/examplefile
VOLUME ["/steamcmd"]

# Install steamcmd and verify that it is working
RUN wget http://media.steampowered.com/installer/steamcmd_linux.tar.gz
RUN tar -xf steamcmd_linux.tar.gz -C /steamcmd
RUN chmod +x /steamcmd/steamcmd.sh
RUN /steamcmd/steamcmd.sh +login anonymous +quit
