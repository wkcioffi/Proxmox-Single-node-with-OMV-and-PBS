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

## Installazione

- Installare **Docker**
- Creare una cartella in cui ospitare i progetti:

    es. */home/user/**docker**/*

- Creare una sottocartella per ogni progetto, contenente il relativo docker-compose file 

## Servizi

- [NGIX Proxy Manager - NPM]()
- [Opencloud](/proxmox-single-node-with-omv-and-pbs/services/opencloud-compose/opencloud-compose.md)
- [Passbolt]()
- [Wireguard]()