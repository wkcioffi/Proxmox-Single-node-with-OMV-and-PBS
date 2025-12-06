# Proxmox Backup Server

## Creazione PBS VM

*   Passthrough del disco USB 2TB alla VM (da GUI)
    
*   Installazione Proxmox Backup Server con disco OS su datastore locale del nodo
    
## Configurazione datastore su disco USB
    
*   Creare il filesystem:

    *   PBS → Storage / Disks → Directory → Create: *Directory*
 
    *   Device: selezionare il disco (es. **/dev/sdb**)
 
    *   Filesystem: XFS va bene per dati di grandi dimensioni come i dischi delle VM
 
    *   Add as datastore: true
 
    *   Removable datastore: true
    
*   Il nuovo filesystem verrà aggiunto come datastore di backup

## Collegamento datastore a PVE

*   Datastore > Storage > Add... > Proxmox Backup Server

    *   ID= *nome del datastore su PVE*

    *   Server= *IP o FQDN del PBS*

    *   Username e password= *credenziali del PBS*

    *   Datastore= *nome del datastore su PBS*

    *   Fingerprint= *recuperarlo da PBS > Dashboard > Show Fingerprint*

*   Content= backup 

## Configurazione backup VM

*   Datastore > Backup > Add

*   Creare policy di backup a piacimento, puntando al datastore del PBS