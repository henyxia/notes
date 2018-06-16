FIRST THING
===========

On mon
------

We absolutely need to check that this new OSD have proper access to the ceph
cluster. Run `ceph -s` and it must return the cluster health status. If not,
the configuration files and keyring may not be synced properly.

Let's go!
=========

On mon
------

* Generate UUID: `UUID=$(uuidgen)`
* Generate the OSD secret: `OSD_SECRET=$(ceph-authtool --gen-print-key)`
* Set ID if the OSD is replacing an decommissioned ID: `ID={my_id}`
* Output them for the next steps: `echo -e "UUID: $UUID\nSECRE: $OSD_SECRET\nID: $ID"`
* Create the new OSD: `echo "{\"cephx_secret\": \"$OSD_SECRET\"}" | ceph osd new $UUID $ID -i - -n client.bootstrap-osd -k /var/lib/ceph/bootstrap-osd/ceph.keyring` and remember the outputted ID

On new OSD
----------

* Import UUID: `read UUID`
* Import OSD secret: `read OSD_SECRET`
* Import OSD ID: `read ID`
* Set the target disk: `read DISK` with a path like `/dev/sdX`
* Import Cluster ID if not already present in your env: `read CLUSTER`
* Check before continuing: `echo -e "UUID: $UUID\nSECRE: $OSD_SECRET\nID: $ID\nCLUSTER: $CLUSTER\nDISK: $DISK"`
* Seems good?

* Format the target disk: `mkfs.xfs $DISK`
* Get the UUID of this new disk: `blkid |grep $DISK` and `read DISK_UUID` or `DISK_UUID=$(blkid |grep $DISK |cut -d'"' -f2)`
* Add this new disk to the fstab: `echo "UUID=$DISK_UUID /var/lib/ceph/osd/$CLUSTER-$ID xfs rw,relatime,attr2,inode64,noquota 0 0" >> /etc/fstab`
* Create the mount point: `mkdir /var/lib/ceph/osd/$CLUSTER-$ID`
* Mount the disk: `mount -a`
* Seems good?

* Write the the OSD secret: `ceph-authtool --create-keyring /var/lib/ceph/osd/$CLUSTER-$ID/keyring --name osd.$ID --add-key $OSD_SECRET`
* Initialize the OSD data directory: `ceph-osd -i $ID --cluster $CLUSTER --mkfs --osd-uuid $UUID`
* Fix ownership: `chown -R ceph:ceph /var/lib/ceph/osd/$CLUSTER-$ID`
* Enable and start the service `systemctl enable ceph-osd@$ID` and finally `systemctl start ceph-osd@$ID`
* The OSD should now join the cluster
