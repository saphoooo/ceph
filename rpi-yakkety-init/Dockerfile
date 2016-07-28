FROM de13/rpi-yakkety
MAINTAINER de13 <stephane.beuret@data-essential.com>

#ENV container docker
#ENV LC_ALL C
ENV DEBIAN_FRONTEND noninteractive
RUN echo 'APT::Install-Recommends "0"; \n\
    APT::Get::Assume-Yes "true"; \n\
    APT::Get::allow-remove-essential "true"; \n\
    APT::Install-Suggests "0";' > /etc/apt/apt.conf.d/01buildconfig && \
    DEBIAN_FRONTEND=noninteractive apt-get update && \
    apt-get install -y apt-utils init-system-helpers openssl sysvinit-utils
#WORKDIR /
#ENV init /sbin/init
