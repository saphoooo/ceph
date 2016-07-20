# Ceph on rpi

## kviator, confd and etcd
**[kviator]** (https://github.com/AcalephStorage/kviator/) is built as mentioned on Github.
**[confd]** (https://github.com/kelseyhightower/confd) is also built as mentioned on Github.
**etcd**:

```
mkdir build ; cd build
wget https://github.com/coreos/etcd/archive/v2.3.0.tar.gz
tar xzf v2.3.0.tar.gz
cd etcd-2.3.0/
./build
cp bin/* /usr/local/bin/.
```

## rpi-ceph-base and rpi-ceph-daemon

only use de13/rpi-ceph-daemon

## How it's work

See this [video] (https://www.youtube.com/watch?v=FUSTjTBA8f8&feature=youtu.be) on youtube.
