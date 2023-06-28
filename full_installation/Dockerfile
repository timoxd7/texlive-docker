# Base Image
FROM ubuntu:20.04

# Non-Interactive
ENV DEBIAN_FRONTEND=noninteractive 

# Timezone Setting
ENV TZ=America/New_York
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

# Install Dependencies
RUN apt-get update && apt-get install -y \
    perl \
    wget \
    python3.8 \
    python3-pip \
    python3-pygments \
    curl \
    inkscape

# Install texlive
RUN cd /tmp \
    && wget https://mirror.ctan.org/systems/texlive/tlnet/install-tl-unx.tar.gz \
    && tar -xzf install-tl-unx.tar.gz \
    && rm install-tl-unx.tar.gz \
    && cd install-tl-* \
    && perl ./install-tl --no-interaction \
    && rm -rf /tmp/*

ENV PATH="/usr/local/texlive/2023/bin/x86_64-linux:${PATH}"
