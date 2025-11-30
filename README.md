# Proxmox-Single-node-with-OMV-and-PBS

1\. Panoramica Architetturale
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
        

2\. Prerequisiti
----------------

*   USB3 o SATA compatibili con passthrough VM
    
*   Immagine ISO Proxmox VE più recente
    
*   Immagini ISO/Template per NAS e PBS (es. TrueNAS CORE, Proxmox Backup Server)
    
*   Cavi e adattatori per dischi interni/esterni
    
*   Accesso a rete e internet per aggiornamenti
    

3\. Installazione Proxmox VE
----------------------------

1.  **Preparazione USB di installazione**
    
    *   Scarica Proxmox VE ISO dal sito ufficiale
        
    *   Creare USB bootable con balenaEtcher o Rufus
        
2.  **Installazione su NVMe**
    
    *   Boot da USB
        
    *   Seleziona NVMe 128GB come disco di destinazione
        
    *   Configura hostname, password e rete
        
3.  apt update && apt full-upgrade -y
    
4.  **Configurazione rete**
    
    *   Assicurati che il nodo abbia IP statico
        
    *   Configura /etc/hosts per risolvere hostname locale
        

4\. Creazione delle VM
----------------------

### 4.1 NAS (OMV) VM

*   Passaggio disco SATA in passthrough
    
*   Installazione di OpenMediaVault (OVM)
    
*   Creazione del volume su HDD SATA
    
*   Esportazione NFS/Block Volume verso Proxmox
    
*   Configurazione utenti/permessi
    

### 4.2 PBS VM

*   Passaggio disco USB 2TB in passthrough
    
*   Installazione Proxmox Backup Server
    
*   Creazione datastore sul disco USB
    
*   Configurazione backup schedulato di tutte le VM
    

### 4.3 VM/LXC di servizio

*   Creazione template VM/LXC su NVMe
    
*   Configurazione repository VM sul NAS (NFS/Block)
    

5\. Configurazione Repository Proxmox
-------------------------------------

1.  Datacenter → Storage → Add → NFS
    
    *   Path: IP\_NAS:/mnt/nas\_volume
        
    *   Abilitare VM Disk, ISO, Backup
        
2.  **Aggiungere datastore per PBS**
    
    *   PBS → Datastore → Add → Path (disco USB passthrough)
        
    *   Configurare backup jobs per tutte le VM
        

6\. Backup e Ripristino
-----------------------

*   **Backup automatico**: PBS → backup job → VM target
    
*   **Ripristino VM**
    
    *   Dal PBS → Restore → selezionare datastore e VM
        
*   **Gestione dischi guasti**
    
    1.  **Disco USB PBS guasto**
        
        *   Sostituire disco
            
        *   Reinstallare PBS VM
            
        *   Riagganciare backup precedente
            
    2.  **Disco SATA NAS guasto**
        
        *   Sostituire disco
            
        *   Reinstallare NAS VM
            
        *   Ripristinare volume dal backup PBS
            

7\. Monitoraggio Hardware
-------------------------

*   Installare VM CheckMK o simile
    
*   Monitoraggio:
    
    *   Stato CPU/RAM
        
    *   Stato dischi NVMe/SATA/USB
        
    *   Temperature e log sistema
        
*   Configurare alert email per guasti
    

8\. Troubleshooting Comuni
--------------------------

ProblemaPossibile causaSoluzioneVM NAS non vede HDD SATAPassthrough non configuratoVerificare qm set VMID -scsiX /dev/disk/by-id/...Backup PBS non parteDisco USB non montatoVerificare passthrough, montaggio, permessiNFS storage non raggiungibileNAS VM non avviata o reteAvviare NAS VM, controllare firewall, test mount -v NFS\_IP:/path /mnt/testVelocità dischi lentaUSB3/VirtualizzazioneVerificare driver USB, limitazioni VM, usare NVMe per template VMAggiornamento Proxmox fallisceRepo non raggiungibiliControllare connessione, /etc/apt/sources.list.d/pve-enterprise.list

9\. Consigli pratici
--------------------

*   Mantenere snapshot VM critiche prima di aggiornamenti
    
*   Testare ripristino backup almeno una volta al mese
    
*   Tenere un disco USB/PBS spare pronto per sostituzioni rapide
    
*   Documentare sempre quali VM/esportazioni sono su quali dischi
