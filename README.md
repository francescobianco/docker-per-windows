# Docker per Windows

Questa guida ha lo scopo di migliorare e ottimizzare le performace di Docker su Windows. 

Le sezioni seguenti presentano dei test delle procedure da eseguire per essere sicuri di avere il miglior risultato possibile

## Verificare e Preparazione WSL

Aprire `cmd.exe` con in privilegi di Administrator è lanciare il seguente comando 

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

![Screenshot from 2023-10-18 17-09-49](https://github.com/francescobianco/docker-per-windows/assets/472171/795bd674-c21d-4ebe-af44-ba0bf92fc7cb)

