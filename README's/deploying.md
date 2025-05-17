# Redeploying docker-compose containers into new proxmox lxc container
## 1. Follow this link to [ProxmoxVE Github](https://github.com/community-scripts/ProxmoxVE) to get the docker lxc script and when deployed dont do auto install instead set to a privleged container
## 2. when you have the lxc go to container options in proxmox gui and enable fuse access
## 3. restart lxc in proxmox
## 4. once lxc is setup make sure your ssh key is in truenas user settings
## 5. update container and install sshfs
```
apt update && apt upgrade
```
```
apt install sshfs -y
```

## 6. make directory for nfs mount that is accessable to everyone
```
cd /
mkdir /data
mkdir /data/sftp
chmod 777 -R /data
```

## 7. give root user ssh private key
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
chmod 600 config && chmod 600 truenas

paste into /etc/fstab
```
truenas-user@truenas-ip:truenas/dir/docker /data/sftp fuse.sshfs _netdev,allow_other,reconnect 0 0
```

## 8. make sure mount is enabled
restart lxc container

check status of mount 
```
systemctl status data-sftp.mount
```

double check by seeing files in sftp mount are listed and check folder permissions
```
ls -la /data/sftp
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
