FROM ubuntu:20.04

MAINTAINER barbaros

ARG DEBIAN_FRONTEND=noninteractive

RUN apt-get update && apt-get install -y tar wget xauth libxdamage-dev qt5-default sudo dialog xdg-utils gtk-update-icon-cache libgl1-mesa-glx libpulse0 libnss3  libxss1 libasound2 libxslt1.1 libxkbcommon-x11-0 libxcb-xinerama0-dev libfreetype6 

RUN export uid=1000 gid=1000 \
  && mkdir -p /home/student \
  && echo "student:x:${uid}:${gid}:student,,,:/home/student:/bin/bash" >> /etc/passwd \
  && echo "student:x:${uid}:" >> /etc/group \
  && mkdir /home/student/storage \
  && mkdir /home/student/Tps \
  && chown ${uid}:${gid} -Rv /home/student
  
COPY packettracer_8.1.1_amd64.deb /
 
RUN dpkg -i /packettracer_8.1.1_amd64.deb \
  && rm -f /*.deb \
  && chown ${uid}:${gid} -Rv /opt/pt
COPY packettracer /usr/local/bin/packettracer

RUN chmod +rx /usr/local/bin/packettracer

RUN apt-get update && apt-get install -y x11vnc xvfb

USER student

ENV HOME /home/student

RUN echo "/usr/local/bin/packettracer" > ~/.xinitrc && chmod +x ~/.xinitrc

CMD ["x11vnc", "-create", "-forever"]
