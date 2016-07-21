# Ceph on rpi

> **Running Ceph Cluster on Raspberry Pi with only one image!**
My aim is to port on Raspberry Pi the excellent work of *Sébastien* Han from **Red Hat** on dockerizing Ceph.

> Some Dockerfiles are rewrited, *kviator*, *confd* and *etcdctl* are builded outside of the Dockerfile, and **Yakkety** is used instead of **Xenial** cause rbd mirror in not available in **Xenial**.

> I still working on this, but reaching the goal!

# Current status

1. **Ceph mon**: OK
1. **Ceph osd**: OK
1. **Ceph mds**: mon crash when mds is started. See `[#16771] (http://tracker.ceph.com/issues/16771)`

# kviator, confd and etcd

[kviator] (https://github.com/AcalephStorage/kviator/) is built as mentioned on Github.

[confd] (https://github.com/kelseyhightower/confd) is also built as mentioned on Github.

for [etcd] (https://github.com/coreos/etcd/archive/):

```
mkdir build ; cd build
wget https://github.com/coreos/etcd/archive/v2.3.0.tar.gz
tar xzf v2.3.0.tar.gz
cd etcd-2.3.0/
./build
cp bin/* /usr/local/bin/.
```

# rpi-ceph-daemon

debootstrap was used to bootstrap rpi-yakkety

```
# debootstrap --variant=buildd --arch=armhf yakkety ./yakkety
# tar  -C yakkety -c . | docker import - rpi-yakkety
```

From this base, you have all Dockerfiles.

# How it's work

See this [video] (https://www.youtube.com/watch?v=FUSTjTBA8f8&feature=youtu.be) on youtube.

**Mainly**

 - Start mon on your first Pi:

```
# sudo docker run -d --net=host -v /etc/ceph:/etc/ceph -v /var/lib/ceph:/var/lib/ceph -e MON_IP=${IP} -e CEPH_PUBLIC_NETWORK=${NETWORK}/24 de13/rpi-ceph-daemon mon
```

 - Copy the config on your 2 others Pi:

```
# scp -r /etc/ceph/* /var/lib/ceph/* pi2:.
# scp -r /etc/ceph/* /var/lib/ceph/* pi3:.
```

 - Start mon on Pi2 and Pi3 the same way as Pi1

 - For test you can create loop devices or directory for osd, or use usb keys.

 - Run osd on your 3 Pi's:

```
# docker run -d --net=host -v /etc/ceph:/etc/ceph -v /var/lib/ceph:/var/lib/ceph -v /dev:/dev --privileged=true \
  -e OSD_FORCE_ZAP=1 -e OSD_DEVICE=/dev/loop0 de13/rpi-ceph-daemon osd_ceph_disk
# docker run -d --net=host -v /etc/ceph:/etc/ceph -v /var/lib/ceph:/var/lib/ceph -v /dev:/dev --privileged=true \
  -e OSD_FORCE_ZAP=1 -e OSD_DEVICE=/dev/loop1 de13/rpi-ceph-daemon osd_ceph_disk
```

 - Run mds on Pi1:

```
# sudo docker run -d --net=host -v /etc/ceph:/etc/ceph -v /var/lib/ceph:/var/lib/ceph -e CEPHFS_CREATE=1 de13/rpi-ceph-daemon mds
```
