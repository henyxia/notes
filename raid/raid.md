Managing soft RAID
==================

Create array
------------

```
mdadm --create /dev/mdX --level=X --raid-devices=X /dev/sdX /dev/sdY
```

Delete array
------------

```
mdadm --stop /dev/mdX
mdadm --zero-superblock /dev/sdX
mdadm --zero-superblock /dev/sdY
```

Add disk to array
-----------------

```
mdadm /dev/mdX --add /dev/sdX
```

Set disk as faulty
------------------

```
mdadm /dev/mdX --fail /dev/sdX
```

Remove disk in array
--------------------

```
mdadm /dev/mdX --remove /dev/sdX
```

Change disk quantity in array
-----------------------------

```
mdadm --grow /dev/mdX --raid-devices=X
```

Switch to RW
------------

```
mdadm --readwrite /dev/mdX
```

Show details of array
---------------------

```
mdadm --detail /dev/md0
```

Show arrays status
------------------

```
cat /proc/mdstat
```
