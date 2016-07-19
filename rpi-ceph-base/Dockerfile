FROM rpi-xenial
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
ADD xenial.sources.list /etc/apt/sources.list
RUN apt-get update && \
    apt-get install -y wget unzip libudev1 udev runit uuid-runtime python-setuptools sharutils python3 ceph radosgw
ADD yakkety.sources.list /etc/apt/sources.list
RUN apt-get update && apt-get install -y rbd-mirror && \
    apt-get clean && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*
ADD entrypoint.sh /entrypoint.sh
ENTRYPOINT ["/entrypoint.sh"]
