Tuning OSD
==========

My OSD are getting OOM killed
-----------------------------

Sometimes, Ceph OSD use way more memory than expected. Most of the time, the
thumb rule to apply is 1To of storage being equal to 1Go of memory. In some
cases, like recovery or scrubbing, the rule does not work, resulting in a OOM
kill in a memory restrained environment.

When using bluestore, it is possible to reduce the memory used by OSD by tuning
some particular settings through the admin socket of the OSD. Usually, this
socket can be found in: `/var/run/ceph/${CLUSTER}-${OSD_ID}.asok`.

To display the current values of the bluestore cache, use:
`ceph daemon ${OSD_ADMIN_SOCKET} config show | grep bluestore_cache_size`

Example:

```
# ceph daemon /var/run/ceph/b1ad1435-8b7f-4689-b334-f4c69a72e50b-osd.2.asok config show | grep bluestore_cache_size
    "bluestore_cache_size": "0",
    "bluestore_cache_size_hdd": "1073741824",
    "bluestore_cache_size_ssd": "3221225472",
```

Setting a lesser value for the OSD has been reported to fix most of OOM
issues. For example, run this command to set the cache to 512MB:
`ceph daemon ${OSD_ADMIN_SOCKET} config set bluestore_cache_size_hdd 536870912`

How to diag OSD memory usage
----------------------------

`ceph daemon ${OSD_ADMIN_SOCKET} dump_mempools`

With `${OSD_ADMIN_SOCKET}` being `/var/run/ceph/${CLUSTER}-${OSD_ID}.asok`.

---

References:
* [Ceph ML on cache size](http://lists.ceph.com/pipermail/ceph-users-ceph.com/2017-September/020829.html)
* [Ceph ML on memory usage diag](http://lists.ceph.com/pipermail/ceph-users-ceph.com/2017-September/020988.html)
* [Ceph doc on bluestore_cache_size](http://docs.ceph.com/docs/master/rados/configuration/bluestore-config-ref/)
