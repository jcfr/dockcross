FROM dockcross/base:latest
MAINTAINER Declan Zhang <deqzhang@ucdavis.edu>

ARG DEBIAN_FRONTEND=noninteractive

# Release
#ARG LINARO_GCC_VERSION=7.2.1-2017.11
#ARG LINARY_DOWNLOAD_FOLDER=latest
#ARG LINARY_DOWNLOAD_SUBDOMAIN=releases

# Snapshot
ARG LINARO_GCC_VERSION=7.3.1-2018.05-rc1
ARG LINARY_DOWNLOAD_FOLDER=7.3-2018.05-rc1
ARG LINARY_DOWNLOAD_SUBDOMAIN=snapshots

# Download URL
ARG LINARY_DOWNLOAD_BASE_URL=https://${LINARY_DOWNLOAD_SUBDOMAIN}.linaro.org/components/toolchain/binaries/${LINARY_DOWNLOAD_FOLDER}

# Toolchains for little-endian, 64-bit ARMv8 for bare-metal systems
# See https://collaborate.linaro.org/display/TCWGPUB/ARM+and+AArch64+Target+Triples
ARG LINARO_TRIPLE=aarch64-elf

# For default configuration of each 'triples', see https://collaborate.linaro.org/display/TCWGPUB/ARM+and+AArch64+Target+Triples+default+options

RUN \
  #
  # Download and install cross-compiler binaries
  #
  set -x && \
  curl -LO -# ${LINARY_DOWNLOAD_BASE_URL}/${LINARO_TRIPLE}/gcc-linaro-${LINARO_GCC_VERSION}-x86_64_${LINARO_TRIPLE}.tar.xz && \
  mkdir -p /opt/linaro/ && \
  tar xf gcc-linaro-${LINARO_GCC_VERSION}-x86_64_${LINARO_TRIPLE}.tar.xz -C /opt/linaro/ && \
  rm -f gcc-linaro-${LINARO_GCC_VERSION}-x86_64_${LINARO_TRIPLE}.tar.xz && \
  #
  # Install emulator
  #
  apt-get update && \
  apt-get install -y \
    qemu-user \
    qemu-user-static

ENV CROSS_TRIPLE ${LINARO_TRIPLE}
ENV CROSS_PATH /opt/linaro/gcc-linaro-${LINARO_GCC_VERSION}-x86_64_${LINARO_TRIPLE}
ENV CROSS_ROOT ${CROSS_PATH}/bin
ENV AS=${CROSS_ROOT}/${CROSS_TRIPLE}-as \
    AR=${CROSS_ROOT}/${CROSS_TRIPLE}-ar \
    CC=${CROSS_ROOT}//${CROSS_TRIPLE}-gcc \
    CPP=${CROSS_ROOT}/${CROSS_TRIPLE}-cpp \
    CXX=${CROSS_ROOT}/${CROSS_TRIPLE}-g++ \
    LD=${CROSS_ROOT}/${CROSS_TRIPLE}-ld
ENV PATH=$PATH:${CROSS_ROOT}
ENV DEFAULT_DOCKCROSS_IMAGE dockcross/bare-armv8

COPY Toolchain.cmake ${CROSS_ROOT}
ENV CMAKE_TOOLCHAIN_FILE ${CROSS_ROOT}/Toolchain.cmake

# Build-time metadata as defined at http://label-schema.org
ARG BUILD_DATE
ARG IMAGE
ARG VCS_REF
ARG VCS_URL
LABEL org.label-schema.build-date=$BUILD_DATE \
      org.label-schema.name=$IMAGE \
      org.label-schema.vcs-ref=$VCS_REF \
      org.label-schema.vcs-url=$VCS_URL \
      org.label-schema.schema-version="1.0"
