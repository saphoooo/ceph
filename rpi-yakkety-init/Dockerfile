FROM de13/rpi-yakkety
MAINTAINER de13 <stephane.beuret@data-essential.com>

ENV container docker
ENV LC_ALL C
ENV DEBIAN_FRONTEND noninteractive
RUN echo 'APT::Install-Recommends "0"; \n\
    APT::Get::Assume-Yes "true"; \n\
    APT::Get::allow-remove-essential "true"; \n\
    APT::Install-Suggests "0";' > /etc/apt/apt.conf.d/01buildconfig && \
    dpkg-divert --local --rename --add /sbin/initctl && \
    ln -s /bin/true /sbin/initctl
WORKDIR /
ENV init /sbin/init
