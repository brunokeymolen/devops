# Based on:
# https://docs.openvinotoolkit.org/latest/_docs_install_guides_installing_openvino_docker_linux.html
FROM ubuntu:18.04
MAINTAINER Bruno Keymolen

### CHANGE THIS!
### Download openvino toolkit first,
### put it next to the dockerfile and adapt the OPENVINO_TOOLKIT_NAME below if needed
ARG OPENVINO_TOOLKIT_NAME=l_openvino_toolkit_p_2020.1.023.tgz
#ARG OPENVINO_TOOLKIT_NAME=l_openvino_toolkit_p_2019.3.376.tgz

ENV TZ=America/New_York

USER root
WORKDIR /


COPY ${OPENVINO_TOOLKIT_NAME} /tmp/.


RUN useradd -ms /bin/bash openvino && \
    chown openvino -R /home/openvino
ARG DEPENDENCIES="autoconf \
                  automake \
                  build-essential \
                  cmake \
                  cpio \
                  curl \
                  gnupg2 \
                  libdrm2 \
                  libglib2.0-0 \
                  lsb-release \
                  libgtk-3-0 \
                  libtool \
                  python3-pip \
                  udev \
                  unzip"
RUN apt-get update && \
    apt-get install -y --no-install-recommends ${DEPENDENCIES} && \
    rm -rf /var/lib/apt/lists/*

WORKDIR /tmp


ENV INSTALLDIR /opt/intel/openvino

RUN tar -xzf ./*.tgz && \
    cd `ls -d */` && \
    sed -i 's/decline/accept/g' silent.cfg && \
    ./install.sh -s silent.cfg && \
    cd /tmp && rm -rf /tmp/* && \
    $INSTALLDIR/install_dependencies/install_openvino_dependencies.sh



# Get rid of UDEV by rebuilding libusb without UDEV support in the Docker* image
RUN usermod -aG users openvino
RUN cp ${INSTALLDIR}/deployment_tools/inference_engine/external/97-myriad-usbboot.rules /etc/udev/rules.d/ && \
    ldconfig
WORKDIR /opt
RUN curl -L https://github.com/libusb/libusb/archive/v1.0.22.zip --output v1.0.22.zip && \
    unzip v1.0.22.zip && cd libusb-1.0.22 && \
    ./bootstrap.sh && \
    ./configure --disable-udev --enable-shared && \
    make -j4
RUN apt-get update && \
    apt-get install -y --no-install-recommends libusb-1.0-0-dev && \
    rm -rf /var/lib/apt/lists/*
WORKDIR /opt/libusb-1.0.22/libusb
RUN /bin/mkdir -p '/usr/local/lib' && \
    /bin/bash ../libtool   --mode=install /usr/bin/install -c   libusb-1.0.la '/usr/local/lib' && \
    /bin/mkdir -p '/usr/local/include/libusb-1.0' && \
    /usr/bin/install -c -m 644 libusb.h '/usr/local/include/libusb-1.0' && \
    /bin/mkdir -p '/usr/local/lib/pkgconfig' && \
    cd  /opt/libusb-1.0.22/ && \
    /usr/bin/install -c -m 644 libusb-1.0.pc '/usr/local/lib/pkgconfig' && \
    ldconfig

RUN pip3 install --upgrade pip && \
    pip install pip setuptools && \
    pip install Pillow && \
    pip install networkx && \
    pip install defusedxml && \
    pip install --upgrade tensorflow==1.15


WORKDIR /opt/intel/openvino

CMD /bin/bash
