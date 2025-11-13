## got insperation from this guide from the tailscale channel: [Link](https://www.youtube.com/watch?v=CipnV3yx2jw)

## command to compile the ignition file
```
podman run --interactive --rm quay.io/coreos/butane:release \
       --pretty --strict < ucore-autorebase.butane > config.ign
```
## grab live iso from [here](https://fedoraproject.org/coreos/download?stream=stable) and downlaod to proxmox
## create a vm in proxmox using iso and make sure you select a cpu that supports avx i choose host
## then run command to install to vm disk with setup from ignition file in repo
```
sudo coreos-installer install /dev/sda \
    --ignition-url https://raw.githubusercontent.com/ZaCarrie/homelab/refs/heads/main/VirtualMachine/config.ign
```
## swap over to root user and setup mango version of komodo following [https://komo.do/docs/setup/mongo](https://komo.do/docs/setup/mongo) and using the systemd version of the periphery agent from this komodo script [here](https://github.com/moghtech/komodo/tree/main/scripts)
```
sudo -i
```
```
mkdir komodo && cd komodo
```
```
curl -sSL https://raw.githubusercontent.com/moghtech/komodo/main/scripts/setup-periphery.py | python3
```
```
systemctl enable periphery.service
```
```
curl -O https://raw.githubusercontent.com/moghtech/komodo/main/compose/mongo.compose.yaml
```
```
curl -O https://raw.githubusercontent.com/moghtech/komodo/main/compose/compose.env
```
## edit files for settings you want but make sure to in the compose.yaml remove the periphery service section and uncomment this section under the core service
```
    extra_hosts:
      - host.docker.internal:host-gateway
```
```
systemctl enable --now docker.service
```
```
docker compose -p komodo -f ./mongo.compose.yaml --env-file ./compose.env up -d
```
## head over to http://ipofcoreos:9120 and inside of komodo ui change address for local server to https://host.docker.internal:8120 and save and it will be connected to systemd agent
## checking lsblk /var, /sysroot, and /etc are writable root folders and not imunitable so ill transfer all of my docker bind mounts into /etc/docker since i had my folder for docker on my other vm /docker i ran this command to copy over making sure docker does not have trailing slash so that it makes the /etc/docker folder for me
```
scp -r root@ipofothervm:/docker /etc
```
## then i copied over all my komodo vars to the new komodo
## signed into github provider and added homelab repo to repos
## went to syncs and added new resource sync setting source to repo and resource path to komodo.toml that is my root dir of repo and enabled managed saved and click execute sync
## found out that since coreos is using SELinux have to add the :z flag at the end of bind mounts 
