# Ceph on rpi

> **Running Ceph Cluster on Raspberry Pi with only one image!**
My aim is to port on Raspberry Pi the excellent work of *Sébastien Han* from **Red Hat** on dockerizing Ceph.

> Some Dockerfiles are rewrited, *kviator*, *confd* and *etcdctl* are builded outside of the Dockerfile, and **Yakkety** is used instead of **Xenial** cause rbd mirror in not available in **Xenial**.

> I still working on this, but reaching the goal!

# Current status

1. **Ceph mon**: OK
1. **Ceph osd**: OK
1. **Ceph rgw**: OK

# kviator and confd

[kviator] (https://github.com/AcalephStorage/kviator/) is built as mentioned on Github.

[confd] (https://github.com/kelseyhightower/confd) is also built as mentioned on Github.

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

copy /etc/ceph/\* /var/lib/ceph/\* to pi2

copy /etc/ceph/\* /var/lib/ceph/\* to pi3

 - Start mon on Pi2 and Pi3 the same way as Pi1

 - For test you can create loop devices or directory for osd, but it's better to use usb keys.

 - Run osd on your 3 Pi's:

```
# docker run -d --net=host -v /etc/ceph:/etc/ceph -v /var/lib/ceph:/var/lib/ceph -v /dev:/dev --privileged=true \
  -e OSD_FORCE_ZAP=1 -e OSD_DEVICE=/dev/sda de13/rpi-ceph-daemon osd_ceph_disk
```

 - Run Rados Gateway on Pi1:

```
# sudo docker run -d -p 80:80 -v /etc/ceph:/etc/ceph -v /var/lib/ceph:/var/lib/ceph de13/rpi-ceph-daemon rgw
```

 - Test your configuration

```
$ docker exec f3672ff3ad ceph -s
    cluster f690ec35-358e-4f24-a99e-a24a9f9b0932
     health HEALTH_OK
     monmap e3: 3 mons at {ceph1=192.168.100.151:6789/0,ceph2=192.168.100.152:6789/0,ceph3=192.168.100.153:6789/0}
            election epoch 8, quorum 0,1,2 ceph1,ceph2,ceph3
     osdmap e29: 3 osds: 3 up, 3 in
            flags sortbitwise
      pgmap v84: 128 pgs, 9 pools, 2300 bytes data, 174 objects
            108 MB used, 44279 MB / 44387 MB avail
                 128 active+clean
HypriotOS/armv7: root@ceph1 in ~
$ docker exec f3672ff3ad rados lspools
rbd
.rgw.root
default.rgw.control
default.rgw.data.root
default.rgw.gc
default.rgw.log
default.rgw.users.uid
default.rgw.users.keys
default.rgw.meta
HypriotOS/armv7: root@ceph1 in ~
$ docker exec f3672ff3ad radosgw-admin user create --display-name="de13" --uid=de13
2016-07-28 22:11:51.772287 6b857000  0 RGWZoneParams::create(): error creating default zone params: (17) File exists
{
    "user_id": "de13",
    "display_name": "de13",
    "email": "",
    "suspended": 0,
    "max_buckets": 1000,
    "auid": 0,
    "subusers": [],
    "keys": [
        {
            "user": "de13",
            "access_key": "IGN88L3OJ1ECHHVMQ03H",
            "secret_key": "BI6JOCo1sbM6B8OkgPEygi23ZakcmTxovpSTyeDy"
        }
    ],
    "swift_keys": [],
    "caps": [],
    "op_mask": "read, write, delete",
    "default_placement": "",
    "placement_tags": [],
    "bucket_quota": {
        "enabled": false,
        "max_size_kb": -1,
        "max_objects": -1
    },
    "user_quota": {
        "enabled": false,
        "max_size_kb": -1,
        "max_objects": -1
    },
    "temp_url_keys": []
}
```
