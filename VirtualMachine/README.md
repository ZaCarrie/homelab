got insperation from this guide from the tailscale channel: [Link](https://www.youtube.com/watch?v=CipnV3yx2jw)

command to compile the ignition file
```
podman run --interactive --rm quay.io/coreos/butane:release \
       --pretty --strict < ucore-autorebase.butane > config.ign
```