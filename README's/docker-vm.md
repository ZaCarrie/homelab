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
apt install sshfs -y
```
```
cd /
```
```
mkdir /data
```
```
mkdir /data/sftp
```
```
chmod 777 -R /data
```

give root user ssh private key
```
cd ~/.ssh
```
nano config
```
Host *
    IdentityFile ~/.ssh/truenas
```
nano truenas
```
ssh private key here
```
```
chmod 600 config && chmod 600 truenas
```
ssh root@truenas.local
yes to keys
exit

nano /etc/fstab
```
root@truenas.local:/mnt/drivepool/networkshare/docker /data/sftp fuse.sshfs _netdev,allow_other,reconnect 0 0
```
```
reboot now
```
```
ls -la /data/sftp
```
resize sda1 to size of disk https://askubuntu.com/questions/958114/resize-ubuntu-dev-sda1-partition-in-virtualbox-vmdk-when-dev-sda-is-already-la
you have to first resize the partition with the following steps:
sudo parted /dev/sda to enter the prompt "(parted)" as the superuser
resizepart 1 to resize the partition 1
-0 resizes it to the end of the disk. - indicates that it should count from the end of the disk, not the start. This makes -0 the last sector of the disk - which is suitable when you want to make it as big as possible. Step 4:
quit to exit parted
The file system meta information needs to indicate the size of disk, and resize2fs updates this. Thus, after expanding, run resize2fs /dev/sda1.


install docker https://docs.docker.com/engine/install/debian/

install portainer buisness edition https://docs.portainer.io/start/install/server/docker/linux

https://docker-vm:9443/ and make account

