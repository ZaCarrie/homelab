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
install docker https://docs.docker.com/engine/install/debian/

install portainer buisness edition https://docs.portainer.io/start/install/server/docker/linux

https://docker-vm:9443/ and make account

