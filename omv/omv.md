# OpenMediaVault

## Creazione VM

*   Creare VM: 

    *   vCPU= 4 
    *   memoria= 4 GB
    *   disco OS= 16 GB (sul repository locale del nodo PVE)

*   Passthrough del disco SATA da 1 TB alla VM (da CLI)
    
*   Installazione di OpenMediaVault (OVM) da ISO

*   Installazione di plugin aggiuntivi (LVM2, S3, etc...)
    
*   Creazione del volume group su HDD SATA

*   Creazione LV, filesystem e share NFS
    
*   Configurazione utenti/permessi

## Configurazione Repository in PVE

-  Datacenter → Storage → Add → NFS
    
    *   Path= ***IP_NAS**:/export/**nfs-share***
        
    *   Content= VM Disk, ISO
        