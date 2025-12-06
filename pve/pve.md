## Installazione Proxmox VE
----------------------------

1.  **Preparazione USB di installazione**
    
    *   Scaricare Proxmox VE ISO dal sito ufficiale
        
    *   Creare USB bootable con balenaEtcher o Rufus
        
2.  **Installazione su NVMe**
    
    *   Boot da USB
        
    *   Seleziona NVMe 128GB come disco di destinazione
        
    *   Configura hostname, password e rete
        
3.  apt update && apt full-upgrade -y
    
4.  **Configurazione rete**
    
    *   IP statico sul nodo
        
    *   Configurare /etc/hosts per risolvere hostname locale
        