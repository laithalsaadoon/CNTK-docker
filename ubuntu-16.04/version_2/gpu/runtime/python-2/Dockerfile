# Ubuntu 16.04, CUDA 9.0
FROM nvidia/cuda:9.0-runtime-ubuntu16.04

ARG CNTK_VERSION="2.5.1"
LABEL maintainer "MICROSOFT CORPORATION" \
      com.microsoft.cntk.version="$CNTK_VERSION"

ENV CNTK_VERSION="$CNTK_VERSION"

# Install CNTK as the default backend for Keras
ENV KERAS_BACKEND=cntk

RUN apt-get update && apt-get install -y --no-install-recommends \
    # General
        ca-certificates \
        wget \
        sudo \
        build-essential \
        && \
    # Clean-up
    apt-get -y autoremove \
        && \
    rm -rf /var/lib/apt/lists/*

# Get CNTK Binary Distribution
RUN CNTK_VERSION_DASHED=$(echo $CNTK_VERSION | tr . -) && \
    ([ "$CNTK_VERSION" != "2.4" ] || VERIFY_SHA256="true") && \
    CNTK_SHA256="8eebff81ef4111b2be5804303f1254cd20de5911a7678c8e64689e5c288dde40" && \
    wget -q https://cntk.ai/BinaryDrop/CNTK-${CNTK_VERSION_DASHED}-Linux-64bit-GPU.tar.gz && \
    ([ "$VERIFY_SHA256" != "true" ] || (echo "$CNTK_SHA256 CNTK-${CNTK_VERSION_DASHED}-Linux-64bit-GPU.tar.gz" | sha256sum --check --strict -)) && \
    tar -xzf CNTK-${CNTK_VERSION_DASHED}-Linux-64bit-GPU.tar.gz && \
    rm -f CNTK-${CNTK_VERSION_DASHED}-Linux-64bit-GPU.tar.gz && \
    /bin/bash /cntk/Scripts/install/linux/install-cntk.sh --py-version 27 --docker

WORKDIR /root
