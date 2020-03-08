FROM ubuntu:18.04
MAINTAINER Bruno Keymolen

RUN  apt-get update -q

ENV DEBIAN_FRONTEND=noninteractive
ENV TZ=America/New_York

RUN apt-get install -y ca-certificates

RUN  apt-get install -y --no-install-recommends \
      sudo \
      git \
      build-essential \
      clang \
      libopencv-dev \
      xterm

ARG user=hough
RUN useradd -ms /bin/bash $user 
RUN echo "$user:$user" | chpasswd 
RUN adduser $user sudo


USER $user
ENV HOME /home/$user

RUN mkdir -p /home/$user/projects && \
  git clone https://github.com/brunokeymolen/hough.git /home/$user/projects/hough && \
  cd /home/$user/projects/hough && \
  make

RUN echo "cd /home/$user/projects/hough" >> $HOME/.bashrc
RUN echo "echo \"BUILD AND RUN THE LATEST VERSION: \ngit pull && make && ./hough -s ./img/russell-crowe-robin-hood-arrow.jpg -t 195\n\"" >> $HOME/.bashrc

CMD /bin/bash
