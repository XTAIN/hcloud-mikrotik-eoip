# Cloud Init Config

    #cloud-config
    manage_etc_hosts: false
    runcmd:
      - ip route add default via 172.13.0.1
      - echo "nameserver 8.8.8.8" > /etc/resolv.conf
      - curl https://raw.githubusercontent.com/XTAIN/hcloud-mikrotik-eoip/main/connect | ip=10.128.0.10 sh -
