got insperation from this guide from the tailscale channel: [Link](https://www.youtube.com/watch?v=CipnV3yx2jw)

command to compile the ignition file
```
podman run --interactive --rm quay.io/coreos/butane:release \
       --pretty --strict < ucore-autorebase.butane > config.ign
```
grab live iso from https://fedoraproject.org/coreos/download?stream=stable and downlaod to proxmox

create a vm in proxmox using iso and run command to install to vm disk with config in repo
```
sudo coreos-installer install /dev/sda \
    --ignition-url https://raw.githubusercontent.com/ZaCarrie/homelab/refs/heads/main/VirtualMachine/config.ign
```
etup mango version of komodo following 
https://komo.do/docs/setup/mongo

use advanced config to use a config file for komodo that you setup on the vm and mount it to komodo https://komo.do/docs/setup/advanced

cd to komodo docker dir, make mongo/config and mongo/data sub dirs

```
curl -O https://raw.githubusercontent.com/moghtech/komodo/main/compose/mongo.compose.yaml
```
curl -O https://raw.githubusercontent.com/moghtech/komodo/main/compose/compose.env
```
```
curl -O https://raw.githubusercontent.com/moghtech/komodo/main/config/core.config.toml
```
edit files for settings you want set
```
sudo systemctl enable --now docker.service
```

setup user systemd periphery with script https://github.com/moghtech/komodo/tree/main/scripts then remote periphery from komodo docker compose
```
docker compose -p komodo -f ./mongo.compose.yaml --env-file ./compose.env up -d
```
```
systemctl --user enable periphery
```
```
sudo loginctl enable-linger $USER
```
inside of komodo ui change address for local to https://host.docker.internal:8120