# Homelab Documentation and Gitops enviroment
The goal of this repo is to have my homelab documented to where I can easily follow to redeploy in the event of no backup and to have the source of truth for my docker deployments

# Infrastrcture Overview
## Cloud Services Used
- github - for storing documentation and docker-compose for gitops with renovate bot
- tailscale - vpn manager for remote access to services so that i dont have open ports in firewall and does SSO as i have to be signed into tailscale when on external networks to access services or can share the services by inviting them to my tailnet
- cloudflare - public dns records to resolve tailscale ip address when on a external network
- nextdns - dns server for security features like ad blocking, blocking new or mistyped domains, and DoH etc..

## Firewall settings
- all homelab equipment in a HomeLab vlan 
  - all wan traffic is routed through a vpn at firewall level
  - overview of firewall policy for vlan
    - traffic through wan = all external connections allowed
    - traffic between other vlans to homelab = only allow return requests
    - traffic between devices in homelab vlan = all blocked has to be whitelist to allow
- dns settigns
  - for homelab devices, lxc containers, and vms set fixed ip with local dns records as {hostname}.local.device
  - made local dns for *.zacarrie.com -> nginx.local.device
  - all remaining requests to nextdns over DoH

## Hardware and Software Used
- firewall = UDM-SE
- one server running proxmox
  - cpu = Ryzen 5 4600G
  - ram = 32gb
  - storage = 1tb M.2 & 4x4tb hdd
  - networking = 1x GbE
  - proxmox containers and vms
      - docker lxc-1 (unprivileged)
          - tailscale
          - nginx proxy manager
          - portainer
      - docker lxc-2 (privileged)
          - portainer edge container
          - other services
      - truenas vm
      - homeassistant os vm
      - nextcloud aio vm

## Network Map
```
internal           external
│                  │
udm dns            cloudflare dns
│                  │
│                  tailscale vpn
├──────────────────┘
docker lxc-1
│
nginx proxy manager (gives valid tls certs)
│
final destination (other containers, other vms, proxmox host gui)
```

# Steps for deploying homelab
- setup cloud service accounts (tailscale, nextdns, cloudflare, and etc..)
- setup firewall
  - point dns server to nextdns in dhcp
  - make vlan for homelab
  - start setting up local dns rewrites
- proxmox
  - setup proxmox on server
  - make truenas vm for cental storage first
  - make other vmz and lxc as see fit 
