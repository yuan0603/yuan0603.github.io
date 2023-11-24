## VMware

* ### share folder
```bash
sudo mkdir /mnt/share

sudo vmhgfs-fuse .host:/Share /mnt/share -o subtype=vmhgfs-fuse,allow_other,uid=1000
```

/etc/fstab:
```bash
.host:/Share	/mnt/share	fuse.vmhgfs-fuse	defaults,allow_other,uid=1000	0	0
```