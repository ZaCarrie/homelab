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