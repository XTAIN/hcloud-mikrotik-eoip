# Cloud Init Config

    #cloud-config
    manage_etc_hosts: false
    runcmd:
      - ip route add default via 172.13.0.1
      - echo "nameserver 8.8.8.8" > /etc/resolv.conf
      - apt update
      - DEBIAN_FRONTEND=noninteractive apt upgrade -y
      - DEBIAN_FRONTEND=noninteractive apt install git ifenslave bridge-utils resolvconf -y
      - cd /opt
      - git clone https://github.com/XTAIN/hcloud-mikrotik-eoip.git xtain-hcloud-network
      - /opt/xtain-hcloud-network/install 10.128.0.15
      - echo "Proxmox Install" # Proxmox Install
      - echo "deb [arch=amd64] http://download.proxmox.com/debian/pve $(. /etc/os-release; printf '%s\n' "$VERSION_CODENAME";) pve-no-subscription" > /etc/apt/sources.list.d/pve-install-repo.list
      - wget https://enterprise.proxmox.com/debian/proxmox-release-$(. /etc/os-release; printf '%s\n' "$VERSION_CODENAME";).gpg -O /etc/apt/trusted.gpg.d/proxmox-release-$(. /etc/os-release; printf '%s\n' "$VERSION_CODENAME";).gpg 
      - DEBIAN_FRONTEND=noninteractive apt update
      - DEBIAN_FRONTEND=noninteractive apt full-upgrade -y
      - DEBIAN_FRONTEND=noninteractive apt install proxmox-default-kernel -y
      - DEBIAN_FRONTEND=noninteractive apt remove os-prober linux-image-amd64 'linux-image-6.1*' -y 
      - update-grub
      - echo -e '#!/bin/sh\nrm /etc/cron.d/proxmox-install\nDEBIAN_FRONTEND=noninteractive apt install proxmox-ve postfix open-iscsi chrony -y\n/bin/sh -c "rm /opt/proxmox-install.sh; /sbin/reboot"\n' > /opt/proxmox-install.sh
      - chmod +x /opt/proxmox-install.sh
      - echo '@reboot root /opt/proxmox-install.sh' > /etc/cron.d/proxmox-install
      - reboot



