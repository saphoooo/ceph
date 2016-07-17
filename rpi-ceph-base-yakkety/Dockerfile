FROM de13/rpi-yakkety
MAINTAINER de13 <stephane.beuret@data-essential.com>

# Kernel tweak for rpi
RUN echo -e "vm.swappiness=1\nvm.min_free_kbytes = 32768\nkernel.pid_max = 32768" >> /etc/sysctl.conf

# Add kviator https://github.com/AcalephStorage/kviator/
ADD kviator /usr/local/bin/kviator

# Add confd https://github.com/kelseyhightower/confd
ADD confd /usr/local/bin/confd
RUN mkdir -p /etc/confd/conf.d && mkdir -p /etc/confd/templates

# Add etcdctl https://github.com/coreos/etcd
ADD etcd /usr/local/bin/etcd
ADD etcdctl /usr/local/bin/etcdctl

# Install Ceph
ADD sources.list /etc/apt/sources.list
RUN apt-get update && apt-get install -y wget unzip uuid-runtime python-setuptools udev runit sharutils ceph radosgw rbd-mirror && \
    apt-get clean && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*
ADD entrypoint.sh /entrypoint.sh
ENTRYPOINT ["/entrypoint.sh"]
