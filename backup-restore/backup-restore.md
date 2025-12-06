# Backup e Restore

## Backup 

Backup automatico tramite policy:

PVE > Backup > *nome policy*


## Restore

*   ### Restore VM
    
*   Dal PBS → Restore → selezionare datastore e VM
        
*   ### Gestione dischi guasti
    
    *  **Disco USB PBS guasto**
        
        *   Sostituire disco e aggiornare il passthrough alla VM PBS
            
        *   Da PBS, ricreare datastore di backup e iniziare una nuova catena di backup da zero
            
    *  **Disco SATA NAS guasto**
        
        *   Sostituire disco e aggiornare il passthrough alla VM NAS
            
        *   Ricreare VG, LV e share NFS da zero
            
        *   Ripristinare i dischi delle VM dal backup PBS
        