install deb 12 vm https://community-scripts.github.io/ProxmoxVE/scripts?id=debian-vm
```
id default
type default
size 200
disk cache none
name docker-vm
cpu model kvm64
cpu core 4
ram 8192
bridge default
mac default
vlan blank
mtu blank
start vm when created
storage pool local
```
update vm
```
apt update && apt upgrade -y
```
setup qemu guest agent https://pve.proxmox.com/wiki/Qemu-guest-agent
```
reboot now
```
setup unattended upgrades https://linuxblog.io/how-to-enable-unattended-upgrades-on-ubuntu-debian/
config enable security and updates
```
apt install nfs-common -y
```
```
cd /
```
```
mkdir -p /data/nfs
```
```
chmod 777 -R /data
```
nano /etc/fstab
```
10.10.10.10:/mnt/WDRed4x4TiB/NetworkShare/docker    /data/nfs   nfs    defaults 0 0
```
```
reboot now
```
```
ls -la /data/nfs
```
resize sda1 to size of disk https://askubuntu.com/questions/958114/resize-ubuntu-dev-sda1-partition-in-virtualbox-vmdk-when-dev-sda-is-already-la
you have to first resize the partition with the following steps:
sudo parted /dev/sda to enter the prompt "(parted)" as the superuser
resizepart 1 to resize the partition 1
-0 resizes it to the end of the disk. - indicates that it should count from the end of the disk, not the start. This makes -0 the last sector of the disk - which is suitable when you want to make it as big as possible. Step 4:
quit to exit parted
The file system meta information needs to indicate the size of disk, and resize2fs updates this. Thus, after expanding, run resize2fs /dev/sda1.


install docker https://docs.docker.com/engine/install/debian/

