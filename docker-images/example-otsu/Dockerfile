#https://www.howtoforge.com/tutorial/how-to-create-docker-images-with-dockerfile/
#as an example we run the otsu project with an opencv dependency

FROM ubuntu:18.04
MAINTAINER Bruno Keymolen

RUN  apt-get update -q

#https://serverfault.com/questions/949991/how-to-install-tzdata-on-a-ubuntu-docker-image
#(this is also possible with a docker-compose.yml file)
ENV DEBIAN_FRONTEND=noninteractive
ENV TZ=America/New_York

RUN apt-get install -y ca-certificates

RUN  apt-get install -y --no-install-recommends \
      sudo \
      git \
      build-essential \
      clang \
      qt5-default \
      libopencv-dev \
      xterm

#add user and make sudouser
RUN useradd -ms /bin/bash developer 
RUN echo "developer:developer" | chpasswd 
RUN adduser developer sudo

USER developer
ENV HOME /home/developer

RUN mkdir -p /home/developer/projects && \
  git clone https://github.com/brunokeymolen/otsu.git /home/developer/projects/otsu && \
  cd /home/developer/projects/otsu && \
  make

#CMD xterm
#CMD ./otsu -s img/harewood.jpg
CMD /bin/bash

#expose some port
EXPOSE 3000



