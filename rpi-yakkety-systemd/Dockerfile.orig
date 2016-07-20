FROM de13/rpi-yakkety
MAINTAINER de13 <stephane.beuret@data-essential.com>

ENV container docker
ENV LC_ALL C
ENV DEBIAN_FRONTEND noninteractive
RUN echo 'APT::Install-Recommends "0"; \n\
    APT::Get::Assume-Yes "true"; \n\
    APT::Get::allow-remove-essential "true"; \n\
    APT::Install-Suggests "0";' > /etc/apt/apt.conf.d/01buildconfig && \
    apt-get update && apt-get install systemd && \
    apt-get clean && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*
WORKDIR /lib/systemd/system/sysinit.target.wants
RUN ls | grep -v systemd-tmpfiles-setup | xargs rm -f $1 && \
    rm -f /lib/systemd/system/multi-user.target.wants/* && \
    rm -f /etc/systemd/system/*.wants/* && \
    rm -f /lib/systemd/system/local-fs.target.wants/* && \
    rm -f /lib/systemd/system/sockets.target.wants/*udev* && \
    rm -f /lib/systemd/system/sockets.target.wants/*initctl* && \
    rm -f /lib/systemd/system/basic.target.wants/* && \
    rm -f /lib/systemd/system/anaconda.target.wants/* && \
    rm -f /lib/systemd/system/plymouth* && \
    rm -f /lib/systemd/system/systemd-update-utmp*
WORKDIR /
ENV init /lib/systemd/systemd
VOLUME [ "/sys/fs/cgroup" ]
ENTRYPOINT ["/lib/systemd/systemd"]
