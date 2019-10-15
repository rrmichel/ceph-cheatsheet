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
| `ceph osd pool application enable backuo cephfs` | set `cephfs` as application on the pool backup |

# pgs

| command | description |
|-|-|
| `` ||

# osds

| command | description |
|-|-|
| `` ||

# cephx

| command | description |
|-|-|
| `` ||

# rbd

| command | description |
|-|-|
| `` ||