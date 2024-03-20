# Docker per Windows

Questa guida ha lo scopo di migliorare e ottimizzare le performace di Docker su Windows. 

Le sezioni seguenti presentano dei test delle procedure da eseguire per essere sicuri di avere il miglior risultato possibile

## Installare Docker Desktop

Installare Docker per Windows raggiungibile a questo link: [📥 Download](https://docs.docker.com/desktop/install/windows-install/)

Assicurarsi di rispettare questi requisiti

- Avere installato Docker Desktop almeno alla version 4.28 o superiore
- Avere almeno 80GB disponibili nel proprio disco principale
- Avere almeno 16GB di RAM e non avere macchine virtuali avviate nel proprio PC

## Verificare e Preparazione WSL

Aprire `cmd.exe` con in privilegi di amministratore è lanciare il seguente comando 

```
wsl --update
```

Successivamente eseguire il seguente comando

```
wsl --list --online
```

Se nell'output non viene riportato la versione "Ubuntu-22.04" allora seguire le istruzioni di questo link <https://pureinfotech.com/install-windows-subsystem-linux-2-windows-10/>

Se tutto è ok, lanciare i seguenti comandi

```
wsl --set-default-version 2
```

```
wsl --install -d Ubuntu-22.04
```

```
wsl --set-version Ubuntu-22.04 2
```

```
wsl --setdefault Ubuntu-22.04
```

```
wsl --list --verbose
```

Se all'ultimo comando vediamo come output qualcosa di simile, possiamo considerare questo passggio completato

```
  NAME                   STATE           VERSION
  docker-desktop-data    Running         2
  docker-desktop         Running         2
* Ubuntu-22.04           Stopped         2
```

## Attivare WSL 2 su Docker Desktop

Andare nei Settings di Docker Desktop e impostare i flag come mostrato nelle seguenti immagini

<img alt="Screenshot from 2023-10-18 17-09-49" src="https://github.com/francescobianco/docker-per-windows/assets/472171/795bd674-c21d-4ebe-af44-ba0bf92fc7cb" width="100%">

<img alt="Screenshot from 2023-10-18 17-18-18" src="https://github.com/francescobianco/docker-per-windows/assets/472171/10816f02-dd9d-4e8c-844e-32e0fca032d6" width="100%">

Dopo aver impostato tutto come indicato cliccare su "Apply & Restart". Poi chiudere Docker Desktop completamente (non lasciarlo in background) e verificare riaprendolo che tutto sia rimasto impostato come indicato in precedenza.

## Velocità dei dischi

In ambito sviluppo di fa spesso uso della funzione di mount dei volumi locali per creare ambienti di sviluppo, questo richiede un buon accesso alla funzionalità di lettura e scrittura del disco, copiare il seguente comando ed fare delle considerazioni sui risultati ottenuti

```
docker run --rm -v %CD%/data:/iops/data tooldockers/iops --randrepeat=1 --ioengine=libaio --direct=1 --gtod_reduce=1 --name=test --filename=test --bs=4k --iodepth=64 --size=4G --readwrite=randrw --rwmixread=75
```

Se i valori ripotati sono simili a riportati sotto o migliori tutto potrebbe andare bene se invece sono peggiori bisognera considerare di cambiare disco o addirittura sistema operativo, passando a Ubuntu direttamente

```
Run status group 0 (all jobs):
   READ: bw=194MiB/s (204MB/s), 194MiB/s-194MiB/s (204MB/s-204MB/s), io=3070MiB (3219MB), run=15802-15802msec
  WRITE: bw=64.9MiB/s (68.1MB/s), 64.9MiB/s-64.9MiB/s (68.1MB/s-68.1MB/s), io=1026MiB (1076MB), run=15802-15802msec
```

