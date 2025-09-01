Archiving this now that the build system has been migrated.  For any things that are running with tailscale, [I have moved to using a container like so](https://github.com/lab-astr0rack-net/core/blob/main/ansible/host_vars/vyos.yml):
```
  - name: tailscale
    image: docker.io/tailscale/tailscale:latest
    network: allow-host-networks
    privileged: 'true'
    uid: '0'
    devices:
      - name: tun
        source: /dev/net/tun
        destination: /dev/net/tun
    capabilities:
      - net-admin
    environment:
      - key: TS_AUTHKEY
        value: "{{ auth_key }}"
      - key: TS_HOSTNAME
        value: "{{ hostname }}"
      - key: TS_ROUTES
        value: "{{ routes }}"
      - key: TS_STATE_DIR
        value: /var/lib/tailscale
      - key: TS_USERSPACE
        value: "false"
    volumes:
      - name: state
        destination: /var/lib/tailscale
        source: /config/user-data/tailscale
```

This functionally is the same as before and works with firewall after being configured.  If you need netbird or similar, I think it works in a similar way but cannot confirm.

# VyOS Builds
Custom VyOS 1.5 Rolling builds built with Github Actions (re-built monthly).

Built with support for cloud-init via the qcow2 images.

## Flavors:
  - Vanilla
    - Just VyOS
  - Tailscale
    - VyOS 1.5 w/ tailscale
  - Netbird
    - VyOS 1.5 w/ netbird
  - Tailscale+Netbird
    - VyOS 1.5 w/ tailscale & netbird  
