FROM ubuntu:20.04
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
      wget \
      make \
      libncurses-dev \
      flex \
      bison \
      gperf \
      python \
      pip \
      tar \
      openssh-client

RUN useradd -ms /bin/bash esp && \
    chown esp -R /home/esp
    
RUN usermod -aG users esp && \
    usermod -aG sudo esp

RUN echo 'esp:password' | chpasswd

RUN ln -sf /usr/bin/python3 /usr/bin/python

#
# Continue as user "esp"
#
USER esp

#Toolchain
#https://docs.espressif.com/projects/esp8266-rtos-sdk/en/latest/get-started/linux-setup.html

RUN cd /home/esp && mkdir toolchain; cd toolchain && \
    wget --no-check-certificate https://dl.espressif.com/dl/xtensa-lx106-elf-gcc8_4_0-esp-2020r3-linux-amd64.tar.gz && \
    tar -xzvf xtensa-lx106-elf-gcc8_4_0-esp-2020r3-linux-amd64.tar.gz

RUN echo "\n\n#ESP8266" >> /home/esp/.bashrc && \
    echo "export PATH=$PATH:/home/esp/toolchain/xtensa-lx106-elf/bin" >> /home/esp/.bashrc

RUN export PATH="$PATH:$HOME/esp/xtensa-lx106-elf/bin" 


#ESP8266_RTOS_SDK
#https://docs.espressif.com/projects/esp8266-rtos-sdk/en/latest/get-started/index.html#get-started-get-esp-idf

RUN cd /home/esp/toolchain && \
    git clone --recursive https://github.com/espressif/ESP8266_RTOS_SDK.git && \
    echo "export IDF_PATH=/home/esp/toolchain/ESP8266_RTOS_SDK" >> /home/esp/.bashrc

RUN export IDF_PATH=/home/esp/toolchain/ESP8266_RTOS_SDK && \
    pip install --upgrade pip && \
    pip install --user -r $IDF_PATH/requirements.txt && \
    cd /home/esp && \
    cp -r $IDF_PATH/examples/get-started/hello_world .

RUN ssh-keygen -q -t rsa -N '' -f ~/.ssh/id_rsa  

RUN echo "\n\n* make menuconfig\n* make" > /home/esp/README.txt
RUN echo "\n\nsudo password for user esp: 'password'\n\n" >> /home/esp/README.txt

RUN echo "\ncat /home/esp/README.txt\n" >> /home/esp/.bashrc

WORKDIR /home/esp/hello_world

CMD /bin/bash 

#expose some port
EXPOSE 8087

