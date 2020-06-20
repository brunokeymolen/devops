FROM ubuntu:18.04
MAINTAINER Bruno Keymolen

ENV TZ=America/New_York

USER root
WORKDIR /

RUN apt-get update
RUN apt-get install -y --no-install-recommends \
      sudo \
      git \
      vim \
      build-essential \
      clang \
      rapidjson-dev \
      libevent-dev \
      libcppunit-dev \
      libboost-all-dev \
      libconfig++-dev \
      libssl-dev

#expose some port
EXPOSE 8087

