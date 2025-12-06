# Troubleshooting Comuni

| Problema | Possibile causa | Soluzione |
|----------|------------------|-----------|
| VM NAS non vede HDD SATA | Passthrough non configurato | Verificare: `qm set VMID -scsiX /dev/disk/by-id/...` |
| Backup PBS non parte | Disco USB non montato | Verificare passthrough, montaggio e permessi |
| NFS storage non raggiungibile | NAS VM non avviata o problema di rete | Avviare la NAS VM, controllare firewall, test: `mount -v NFS_IP:/path /mnt/test` |
| Velocità dischi lenta | USB3 / limiti di virtualizzazione | Verificare driver USB, limitazioni della VM, usare NVMe per template VM |
| Aggiornamento Proxmox fallisce | Repository non raggiungibili | Controllare la connessione e il file: `/etc/apt/sources.list.d/pve-enterprise.list` |
