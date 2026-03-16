## process of mounting truenas nfs share on various vm

# must have nfs client
```
sudo apt update && sudo apt install nfs-common -y
```
# verify share is available
```
showmount -e [ip/hostname]
```

# test mount
```
# create mount point
sudo mkdir -p /mnt/nfs-share (or wherever you want to mount it)

# mount share
sudo mount -t nfs [ip/hostname]:/path/to/share /your/local/mount/point [options]

# my exmaple
sudo mount -t nfs 192.168.8.102:/mnt/pve/truenas-nfs /mnt/nfs-share
```

# permanent mount (for persistence after reboots)
```
# back up /etc/fstab before editing
sudo cp /etc/fstab /etc/fstab.bak

# add entry for nfs to /etc/fstab
sudo vim /etc/fstab

# add line for nfs share
server_ip:/path/to/share /your/local/mount/point nfs [options] 0 0

# my example
192.168.8.102:/mnt/pve/truenas-nfs /mnt/nfs-share nfs vers=4.rw.sync 0 0
```
referenced - https://linuxvox.com/blog/how-to-mount-an-nfs-share-in-linux/
