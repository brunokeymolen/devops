FROM ubuntu:18.04

RUN apt-get update && apt-get install -y firefox

RUN apt-get install -y software-properties-common

#add flash
RUN add-apt-repository "deb http://archive.canonical.com/ubuntu $(lsb_release -sc) partner" && \
    apt-get update && \
    apt install -y adobe-flashplugin


#add user and make sudouser
RUN useradd -ms /bin/bash developer 
RUN echo "developer:developer" | chpasswd 
RUN adduser developer sudo


USER developer
ENV HOME /home/developer
CMD /usr/bin/firefox
