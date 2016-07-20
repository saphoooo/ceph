FROM de13/rpi-yakkety-systemd
MAINTAINER de13 <stephane.beuret@data-essential.com>

# Add kviator https://github.com/AcalephStorage/kviator/
ADD kviator /usr/local/bin/kviator

# Add confd version 0.11.0 https://github.com/kelseyhightower/confd
ADD confd /usr/local/bin/confd
RUN mkdir -p /etc/confd/conf.d && mkdir -p /etc/confd/templates

# Add etcdctl version 2.3.0 https://github.com/coreos/etcd
ADD etcd /usr/local/bin/etcd
ADD etcdctl /usr/local/bin/etcdctl

# Install Ceph
RUN sed -i 's_^deb .*_& universe_' /etc/apt/sources.list
RUN apt-get update && \
    apt-get install -y ceph ceph-base ceph-common ceph-fs-common ceph-mds \
    ceph-mon ceph-osd libcephfs1 python-cephfs runit udev && \
    apt-get clean && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*
ADD entrypoint.sh /entrypoint.sh
ENTRYPOINT ["/entrypoint.sh"]
