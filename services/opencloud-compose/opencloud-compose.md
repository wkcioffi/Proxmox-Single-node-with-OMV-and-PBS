# Opencloud

File sharing con versioning; comodo perché spogliato di tutti i componenti aggiuntivi.

Il servizio è avviato sul server **docker01** tramite compose file.

[Link del progetto](https://docs.opencloud.eu/docs/admin/getting-started/container/docker-compose/docker-compose-base)

[Link del git da clonare](https://github.com/opencloud-eu/opencloud-compose.git) (attenzione, non è il repo git principale ma quello dedicato al compose)

## Configurazione Opencloud

Clonare il repo git nella cartella di destinazione dedicata al servizio:

```/home/```**user**```/docker/opencloud-compose```

Allegato in questo repo, c'è il file [.env](/proxmox-single-node-with-omv-and-pbs/services/opencloud-compose/.env) con i parametri già compilati per avviare l'applicazione. Caricarlo allo stesso percorso degli altri file.

Nel file sarà definita una password di *admin* temporanea da modificare al primo accesso.

Nel file è definito l'avvio del container dietro a ad un reverse proxy esterno (in questo caso, NPM) che si occupera di staccare il certificato:

    :external-proxy/opencloud-exposed.yml

Nel file sono definiti i percorsi locali del server docker che vengono mappati sul container di Opencloud; questi percorsi conterranno i dati degli utenti e saranno persistenti:

    OC_CONFIG_DIR=/home/user/docker/opencloud-compose/config
    OC_DATA_DIR=/home/user/docker/opencloud-compose/data

Campi configurati nel mio file:

    COMPOSE_FILE=docker-compose.yml:external-proxy/opencloud-exposed.yml
    OC_DOCKER_IMAGE=opencloudeu/opencloud
    OC_DOCKER_TAG=4.0.0
    OC_DOMAIN=cloud.dominio.com
    INITIAL_ADMIN_PASSWORD=your-super-secure-password
    OC_CONFIG_DIR=/home/user/docker/opencloud-compose/config
    OC_DATA_DIR=/home/user/docker/opencloud-compose/data


## Configurazione reverse proxy (NPM)

Su NPM, bisogna configurare l'host in questo modo:

- source= *cloud.dominio.com*
- destination= ***http**://**docker-ip-or-fqdn**:**9200***
- block common exploit= *false*
- ssl certificate= *assegnare un certificato nominale o wildcard*
- force ssl= *true*
- http/2 support= *true*
- hsts enabled= *true*
- Custom NP: configuration:
    ```
    proxy_set_header Host $host;
    proxy_set_header X-Real-IP $remote_addr;
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    proxy_set_header X-Forwarded-Proto https;
    ```

## Configurazione dominio

Sul dominio pubblico, bisogna creare il record di dominio (uguale a quello configurato nel file *.env*) che punti all'indirizzo IP pubblico del reverse proxy:
- se il dominio radice punta già all'IP del reverse proxy:

    creare "Record CNAME=**cloud.dominio.com**" > **dominio.com**

- se il dominio radice punta ad un indirizzo diverso dal reverse proxy:

     creare "Record A=**cloud.dominio.com**" > IP pubblico del reverse proxy


## Ripristino

I file di Opencloud vengono salvati principalmente nelle cartelle **./data** e **./config**. Queste cartelle sono fondamentali per il ripristino dell'istanza in caso di disastro perchè contengono i dati effettivi degli utenti (e relativi, indispensabili metadati).

In caso di ripristino, occorre salvare i seguenti contenuti per avviare una nuova istanza con gli stessi dati:

- .env
- ./data
- ./config
