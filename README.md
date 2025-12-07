# Proxmox-Single-node-with-OMV-and-PBS

## Indice

- [Panoramica](#panoramica)
    - [Architettura](#architettura)
    - [Prerequisiti](#prerequisiti)
    - [Consigli pratici](#consigli-pratici)
- [Hardware](/proxmox-single-node-with-omv-and-pbs/hardware/hardware.md)
- [Proxmox Virtual Environment](/proxmox-single-node-with-omv-and-pbs/pve/pve.md)
- Creazione VM
  - [OpenMediaVault](/proxmox-single-node-with-omv-and-pbs/omv/omv.md)
  - [Proxmox Backup Server](/proxmox-single-node-with-omv-and-pbs/pbs/pbs.md)
  - [Pi-hole LXC](/proxmox-single-node-with-omv-and-pbs/services/pihole/pihole.md)
  - [Docker](/proxmox-single-node-with-omv-and-pbs/services/docker.md)
- [Backup e Restore](/proxmox-single-node-with-omv-and-pbs/backup-restore/backup-restore.md)
- [Monitoraggi](/proxmox-single-node-with-omv-and-pbs/monitoring/monitoring.md)

---

## Panoramica

### Architettura
-----------------------------

*   **Host**: HP ProDesk 400 G7 Mini PC
    
    *   CPU: Intel i7 8-core con HyperThreading
        
    *   RAM: 16GB
        
    *   Storage: NVMe 128GB (OS e VM template), HDD 500GB SATA (NAS), Disco esterno USB3 2TB (backup PBS)
        
    *   NIC: 1 Gbps
        
*   **VM su Proxmox**
    
    *   **NAS VM**: con passthrough HDD SATA, esporta NFS o iSCSI a Proxmox
        
    *   **PBS VM**: con passthrough disco USB per backup VM
        
    *   **Altre VM/LXC**: servizi vari
        
    *   **Monitoraggio**: CheckMK o altro sistema di monitoraggio VM
        
*   **Obiettivi**
    
    *   Backup sicuro dei dati delle VM senza fermare la produzione
        
    *   Possibilità di sostituire i dischi SATA/USB senza perdita di dati
        
    *   Storage condiviso locale via NFS/block per Proxmox

## Prerequisiti
----------------

*   USB3 o SATA compatibili con passthrough VM
    
*   Immagine ISO Proxmox VE più recente
    
*   Immagini ISO/Template per NAS e PBS (es. OpenMediaVault, Proxmox Backup Server)
    
*   Cavi e adattatori per dischi interni/esterni
    
*   Accesso a rete e internet per aggiornamenti

## Consigli pratici
--------------------

*   Mantenere snapshot VM critiche prima di aggiornamenti
    
*   Testare ripristino backup almeno una volta al mese
    
*   Tenere un disco USB/PBS spare pronto per sostituzioni rapide
    
*   Documentare sempre quali VM/esportazioni sono su quali dischi
