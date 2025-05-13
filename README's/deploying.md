# Redeploying docker-compose containers into new proxmox lxc container
## 1. Follow this link to [ProxmoxVE Github](https://github.com/community-scripts/ProxmoxVE) to get the docker lxc script and when deployed dont do auto install instead set to a privleged container
## 2. when you have the lxc go to container options in proxmox gui and enable nfs mount
## 3. restart lxc in proxmox
## 4. once lxc is setup make sure your nfs server acl's and whitelisted ip is correct
## 5. update container and install nfs tools
```
apt update && apt upgrade
```
```
apt install nfs-common
```

## 6. make directory for nfs mount that is accessable to everyone
```
cd / $$ mkdir /data && mkdir /data/nfs && chmod 777 -R /data
```

## 7. make systemd nfs mount file
```
nano /etc/systemd/system/data-nfs.mount
```

paste into data-nfs.mount file created with nano
```
[Unit]
Description=truenas nfs share
After=network.target

[Mount]
What=0.0.0.0:/mnt/Portainer
Where=/data/nfs
Type=nfs
Options=_netdev,auto

[Install]
WantedBy=multi-user.target
```

## 8. test and enable nfs mount
load made data-nfs.mount into systemd
```
systemctl daemon-reload
```

start the mount to test
```
systemctl start data-nfs.mount
```

see if mount is active and mounted
```
systemctl show -p ActiveState -p SubState --value data-nfs.mount
```

double check by seeing files in nfs share are listed and check folder permissions
```
ls -la /data/nfs
```

if everything checks out enable the mount so persistent after reboot
```
systemctl enable data-nfs.mount
```

## 9. in portainer go to stacks and deploy from git a stack for each service folder in this repo with these settigns
```
Stack name = name of contaienr
build method = Git repository
Authentication = enabled
git username = github username
Personal Access Token = from here https://github.com/settings/tokens
You can use the URL of a git repository.
Repository URL = https://github.com/portainer/portainer-compose
Skip TLS Verification = disabled
Repository reference = default "refs/heads/main"
Compose path = /dontainerfolder/docker-compose.yml
Additional paths and enviroment variables = blank
Access control = disabled
```
