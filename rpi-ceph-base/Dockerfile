FROM de13/rpi-yakkety-init
MAINTAINER de13 <stephane.beuret@data-essential.com>

# Add kviator https://github.com/AcalephStorage/kviator/
ADD kviator /usr/local/bin/kviator

# Add confd version 0.11.0 https://github.com/kelseyhightower/confd
ADD confd /usr/local/bin/confd
RUN mkdir -p /etc/confd/conf.d && mkdir -p /etc/confd/templates && \

# Install Ceph
    sed -i 's_^deb .*_& universe_' /etc/apt/sources.list && \
    DEBIAN_FRONTEND=noninteractive apt-get update && apt-get dist-upgrade -y && \
    apt-get install -y ceph ceph-base ceph-common ceph-fs-common ceph-mds lsb-release ceph-mon ceph-osd\
    radosgw libcephfs1 python-cephfs runit udev python-setuptools sharutils python3 etcd && \
    apt-get clean && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

ADD entrypoint.sh /entrypoint.sh
ENTRYPOINT ["/entrypoint.sh"]
