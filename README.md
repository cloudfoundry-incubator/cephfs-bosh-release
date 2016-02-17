# ceph-boshrelease

*Tested on bosh-lite only*

## To Install
```
git clone https://github.com/cloudfoundry-incubator/cephfs-bosh-release.git
bosh create release
```
Accept the default name for the release (cephfs-bosh-release)

```
bosh upload release
bosh deploy
```
When prompted, answer `yes`.

SSH onto the bosh release VM

`bosh ssh --gateway_host 192.168.50.4 --gateway_user vcap --strict_host_key_checking=no cephfs/0`

Check the health of the ceph-cluster

`ceph -s`

which should report something like this:-

```
cluster c0162a84-1d21-46a2-8a8e-4507f6ec707f
 health HEALTH_OK
 monmap e1: 1 mons at {f4fb115c-1ec1-4788-8a14-8c76f98a9545=10.244.8.2:6789/0}
        election epoch 2, quorum 0 f4fb115c-1ec1-4788-8a14-8c76f98a9545
 mdsmap e5: 1/1/1 up {0=15557bd5-7880-46d0-b33d-fac8b420a65f=up:active}
 osdmap e15: 3 osds: 3 up, 3 in
        flags sortbitwise
  pgmap v20: 320 pgs, 3 pools, 1960 bytes data, 20 objects
        51265 MB used, 151 GB / 206 GB avail
             320 active+clean
```

## Mount cephfs (using fuse)

```
sudo mkdir ~/mycephfs
sudo ceph-fuse -k /etc/ceph/ceph.client.admin.keyring -m 10.244.8.2:6789 ~/mycephfs
```

Check the volume is mounted

`df -h`

and test the mount

```
pushd mycephfs
sudo mkdir mydir
sudo sh -c 'echo "something" > myfile'
cat myfile
```
