# Guida alla Configurazione del Nodo zkSync Era

Questa guida spiega passo dopo passo come configurare un nodo `zkSync Era` su Ubuntu Linux.

## Requisiti di Sistema

- CPU: Si consiglia una CPU relativamente moderna.
- RAM: 32 GB
- Archiviazione: 300 GB, con una crescita dello stato di circa 1 TB al mese.
- Rete: Connessione da 100 Mbps (si consiglia 1 Gbps+)

### Noleggio del Server

Se hai bisogno di un server, puoi noleggiarne uno da Hetzner. Inoltre, se ti registri utilizzando questo link di riferimento, riceverai un credito di 20 EUR.

[Hetzner](https://hetzner.cloud/?ref=fu2umOyLCWhh)

## Passaggio 1: Installazione di Visual Studio Code (Sul Computer Locale)

1. **Scaricare e Installare Visual Studio Code:**
   [Pagina di Download di Visual Studio Code](https://code.visualstudio.com/)

   - Una volta scaricato, segui la procedura guidata di installazione per installare Visual Studio Code.

2. **Installare l'Estensione SSH:**
   - Apri Visual Studio Code.
   - Clicca sull'icona delle `Estensioni` nella barra laterale sinistra.
   - Digita `Remote - SSH` nella barra di ricerca e installa l'estensione sviluppata da Microsoft.

## Passaggio 2: Connessione al Server tramite SSH (Dal Computer Locale)

1. **Connettersi a SSH in Visual Studio Code:**

   - Premi `Ctrl+Shift+P` in Visual Studio Code e seleziona `Remote-SSH: Connect to Host...`.
   - Inserisci l'indirizzo del tuo server nel formato `ssh root@indirizzo_ip`.
   - Inserisci la tua password o chiave SSH per completare la connessione.

   **Nota:** Sostituisci `indirizzo_ip` con l'indirizzo IP reale del tuo server.

2. **Verifica della Connessione:**

   - Se la connessione è riuscita, il nome del server connesso apparirà nell'angolo in basso a sinistra della finestra di Visual Studio Code.

3. **Aprire il Terminale:**
   - Dopo aver connesso il server in Visual Studio Code, puoi aprire il terminale cliccando su `Terminale` > `Nuovo Terminale` nel menu in alto.
   - In alternativa, puoi aprire il terminale premendo `Ctrl+` (tasto Control e il tasto del backtick, situato sotto il tasto ESC).

## Passaggio 3: Installazione di Docker e Docker Compose (Sul Server)

Dopo esserti connesso con successo al server, esegui questi comandi sul server.

### Installazione di Docker

1. **Aggiornare i Pacchetti di Sistema:**

   ```sh
   sudo apt update
   ```

2. **Installare Docker:**

   ```sh
   sudo apt install docker.io
   ```

3. **Verificare che il Servizio Docker sia in Esecuzione:**

   ```sh
   sudo systemctl status docker
   ```

4. **Verificare l'Installazione di Docker:**
   ```sh
   docker --version
   ```

### Installazione di Docker Compose

1. **Scaricare Docker Compose:**

   ```sh
   sudo curl -L "https://github.com/docker/compose/releases/download/1.29.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
   ```

2. **Concedere il Permesso di Esecuzione a Docker Compose:**
   ```sh
   sudo chmod +x /usr/local/bin/docker-compose
   ```
3. **Verificare l'Installazione:**
   ```sh
   docker-compose --version
   ```

## Passaggio 4: Avviare il Nodo zkSync Era (Sul Server)

1. **Installare Git (se non già installato):**

   ```sh
   sudo apt install git
   ```

2. **Clonare il Repository:**

   ```sh
   git clone https://github.com/matter-labs/zksync-era.git
   ```

3. **Navigare nelle Directory Clonate:**

   ```sh
   cd zksync-era/docs/guides/external-node
   cd docker-compose-examples
   ```

4. **Avviare il Nodo Usando Docker Compose:**

   ```sh
   docker-compose -f mainnet-external-node-docker-compose.yml up -d
   ```

5. **Verificare i Contenitori in Esecuzione:**

   ```sh
   docker ps
   ```

   Questo comando elenca i contenitori Docker attualmente in esecuzione. Dovresti vedere i contenitori relativi al nodo `zkSync Era`.

6. **Visualizzare e Seguire gli Ultimi 100 Log:**
   ```sh
   docker logs -f --tail 100 docker-compose-examples-external-node-1
   ```

Premi `Ctrl+C` per uscire dalla visualizzazione dei log. Questo non fermerà il tuo nodo; continuerà a funzionare in background.

## Passaggio 5: Accesso alla Dashboard tramite Forwarding delle Porte (Sul Computer Locale)

Dopo aver eseguito Docker, puoi accedere a una dashboard sulla porta 3000. Per visualizzare questa dashboard dal tuo computer locale, devi configurare il forwarding delle porte in Visual Studio Code.

1. **Configurare il Forwarding delle Porte:**

   - Premi `Ctrl+Shift+P` per aprire la palette dei comandi.
   - Digita `Forward a Port` e seleziona l'opzione.
   - Inserisci `3000` come numero di porta e premi Invio.

   In alternativa:

   - Clicca sulla sezione `PORTS` nell'angolo in basso a destra di Visual Studio Code.
   - Seleziona `Forward Port...` dal menu.
   - Inserisci `3000` come numero di porta e premi Invio.

2. **Accesso alla Dashboard:**

   - Apri il tuo browser web e vai su `http://localhost:3000/d/0/external-node`.
   - Ora dovresti avere accesso alla dashboard.

Questo passaggio ti permette di visualizzare facilmente la dashboard dell'applicazione in esecuzione sul tuo server remoto dal tuo computer locale.

## Informazioni Aggiuntive

- **Visualizzare i Log dei Contenitori:**

  ```sh
  docker logs <container_id>
  ```

- **Arrestare il Nodo:**
  ```sh
  docker-compose -f mainnet-external-node-docker-compose.yml down
  ```
