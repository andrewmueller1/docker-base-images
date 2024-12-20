# Base image
FROM lab/ubuntu:latest

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
        python-pip \
        python3-pip \
        python-ipykernel \
        python3-ipykernel \
        steamcmd \
        lib32gcc1 \
        libstdc++6 \
        libsdl2-2.0-0:i386 \
        libcurl4-openssl-dev:i386 && \
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
RUN wget https://bootstrap.pypa.io/get-pip.py -O /tmp/get-pip.py
RUN python3 /tmp/get-pip.py
RUN pip3 install botocore || /usr/local/bin/pip3 install botocore

# Mount /steamcmd (rust-server) volume
RUN mkdir -p /steamcmd		
RUN touch /steamcmd/examplefile
VOLUME ["/steamcmd"]

# Install steamcmd and verify that it is working
RUN curl -s http://media.steampowered.com/installer/steamcmd_linux.tar.gz \
    | tar -v -C /steamcmd -zx && \
    chmod +x /steamcmd/steamcmd.sh && \
    /steamcmd/steamcmd.sh +login anonymous +quit
