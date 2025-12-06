# Docker

## Creazione VM docker

Creata VM Ubuntu 24.04 LTS

- vCPU= 4
- RAM= 8 GB (2 GB reserved)
- Disks:
    - sda= 16 GB > boot (part) + root (lv)
    - sdb= 2 GB > swap (lv)
    - sdc= 16 GB > /var (lv)
    - sdd= 8 GB > /home (lv)
    - sde= 32 GB > /home/wk/docker + /var/lib/docker (lv)
    - sdf= 32 GB > /home/wk/docker/opencloud-compose (lv)

