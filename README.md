**Still under construction.**

# general

| command | description |
|-|-|
| `ceph -s` ||
| `ceph -w` ||
| `ceph health detail` ||
| `ceph -s -f json` ||
| `ceph df` ||
| `rados df` ||
| `ceph osd tree` ||
| `ceph osd df tree` ||

| command | description |
|-|-|
| `systemctl status ceph-mon@$hostname` ||
| `systemctl status ceph-osd@$osd_id` ||
| `systemctl status ceph-radosgw@$rgw.$hostname` ||

# pools

| command | description |
|-|-|
| `ceph osd pool ls detail` | list all pools incl parameters |
| `ceph osd pool stats` | list all io on the pools |
| `ceph osd pool create data 64` | create a pool `data` with 64 pgs |
| `ceph osd pool create ec_data 64 erasure` | create a pool `ec_data` with 64 pgs with erasure code |
| `ceph osd pool set $name min_size $size` | set the min_size for the pool $name to $size |
| `ceph osd erasure-code-profile set rack84 k=8 m=4 crush-failure-domain=rack` | create a new EC profile with 8+4 and rack as failure domain |
| `ceph osd pool create backup 64 erasure rack84` | create a pool `backup` with 64 pgs with erasure code and `rack84` as profile |
| `ceph osd pool application enable backup cephfs` | set `cephfs` as application on the pool backup |
| `ceph osd pool application set backup cephfs data=cephfs` | set k/v (`data=cephfs`) for application `cephfs` on the pool backup |

# pgs

| command | description |
|-|-|
| `ceph pg stat` ||
| `ceph pg ls` ||
| `ceph pg dump` ||
| `ceph pg $pgid query` ||
| `ceph pg map $pgid` ||
| `ceph pg repair $pgid` ||
| `ceph osd map $poolname $obj-name` ||

#### Object-PG-OSD mapping

![ceph-pg-mapping](images/ceph-pg-osd-mapping.png)

# CRUSH

| command | description |
|-|-|
| `ceph osd crush add-bucket $name $type $args` | Add bucket $name as $type to the crush map to location $args eg. `.. add-bucket rack1 rack datacenter=dc1`|
| `ceph osd crush move $name $type=$name` | Move bucket $name to location $type=$name eg. `.. move rack1 datacenter=dc1`|

# osds

| command | description |
|-|-|
| `ceph osd set $flag` | e.g. noout or norebalance |
| `ceph osd perf` ||
| `ceph osd metadata $osdid` ||
| `ceph osd find $osdid` ||
| `ceph osd down $osdid` ||
| `ceph osd in $osdid` ||
| `ceph osd destroy $osdid` ||
| `ceph osd purge $osdid` ||
| `ceph osd pause|unpause` ||
| `ceph osd blacklist add $clientip` ||
| `ceph osd reweight $osdid $float` | set a reweight (0..1) for a osd - don't mix that with the next cmd (!!!!!^111) |
| `ceph osd crush reweight $osdid $float` | set a new weight for a osd - usally the disk size. |

# cephx

| command | description |
|-|-|
| `ceph auth get-or-create client.$user mon '$mon_caps' osd '$osd_caps' mds '$mds_caps'` ||
| `ceph auth get-key client.$user` | shows only the key for client.$user |
| `ceph auth caps client.$user mon '$mon_caps' osd '$osd_caps' mds '$mds_caps'` | modify the caps for client.$user |

#### mon caps

`allow rw` or `profile rbd`

#### osd caps

`allow [rwx] [namespace=$ns] [pool=$pool_name]`

or

`profile $profile [namespace=$ns] [pool=$pool_name]`

see [cephx profiles](https://docs.ceph.com/docs/master/rados/operations/user-management/#authorization-capabilities)

As a general rule you should always use `profile ...` instead of e.g. `rw` !!

#### mds caps

`allow [rwxs] [path=/] [tag $application $key=$value]`

The `tag` flag corresponds to the `application` of a pool. With key/value you can add your own parameters to a key.

`caps: [osd] allow rw tag cephfs data=cephfs`

The rule above only allow to write/read on pools which have the application `cephfs` set, and additionally the k/v entry `data=cephfs` exists in the application. See **Pools**!

#### Restore a lost `ceph.client.admin.keyring`

`[root@ceph-mon]# ceph -n mon. -k /var/lib/ceph/mon/ceph-$hostname/keyring auth export client.admin`

# rbd

| command | description |
|-|-|
| `rbd create --size $size $name` ||

# ceph-daemons

A daemon is eg.
* mon.$hostname
* osd.*
* osd.$id
* mgr.$hostname
* mds.$hostname

| command | description |
|-|-|
| `ceph tell $daemon $cmd` | eg. `ceph tell osd.1 bench`|
| `ceph tell $daemon injecargs '--parameter=1'` | set a parameter on-the-fly for a daemon |
| `ceph daemon $daemon config show` | access a ceph-daemon via tcp |
| `ceph --admin-daemon $path-to-asok config show` | direct access to a ceph-daemon over a unix socket |