Remove a Ceph OSD
=================

Preflight checklist
-------------------

* Connect on the OSD server and check ceph status `ceph -s`
* Removing an OSD is NOT recommended if the health is not `HEALTH_OK`
* Set the OSD_ID with `export OSD_ID=X`

Kick out the OSD
------------------

* Remove the OSD with `ceph osd out $OSD_ID`
* Check the reweight (should be 0) with `ceph osd tree`

Shut off the OSD
----------------

* Stop the OSD `systemctl stop ceph-osd@${OSD_ID}.service`
* Remove the associated service `systemctl disable ceph-osd@${OSD_ID}.service`

Remove the OSD from the CRUSH map
---------------------------------

* Remove the OSD from the CRUSH map with `ceph osd crush rm osd.$OSD_ID`
* Check the removal with `ceph osd crush tree`

Remove the remaining parts
--------------------------

* Remove the OSD authentication part with `ceph auth del osd.$OSD_ID`
* Check the key removal with `ceph auth list`
* Remove the final data with `ceph osd rm $OSD_ID`
* Remove the mount point (if any) in `/etc/fstab`
* Remove the OSD mount point folder (if any) located in `/var/lib/ceph/osd/$CLUSTER-$ID`

*The OSD should now be removed properly*
