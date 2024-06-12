# zkSync Era Node Einrichtungsanleitung

Diese Anleitung erklärt Schritt für Schritt, wie Sie einen `zkSync Era` Node auf Ubuntu Linux einrichten.

## Systemanforderungen

- CPU: Ein relativ moderner Prozessor wird empfohlen.
- RAM: 32 GB
- Speicher: 300 GB, wobei der Zustand etwa 1 TB pro Monat zunimmt.
- Netzwerk: 100 Mbit/s Verbindung (1 Gbit/s+ empfohlen)

### Servermiete

Falls Sie einen Server benötigen, können Sie einen bei Hetzner mieten. Zusätzlich erhalten Sie einen 20 EUR Guthaben, wenn Sie sich über diesen Empfehlungslink anmelden.

[Hetzner](https://hetzner.cloud/?ref=fu2umOyLCWhh)

## Schritt 1: Visual Studio Code Installation (Auf dem lokalen Computer)

1. **Visual Studio Code herunterladen und installieren:**
   [Visual Studio Code Download-Seite](https://code.visualstudio.com/)

   - Nach dem Download folgen Sie dem Installationsassistenten, um Visual Studio Code zu installieren.

2. **SSH-Erweiterung installieren:**
   - Öffnen Sie Visual Studio Code.
   - Klicken Sie auf das `Erweiterungen`-Symbol in der linken Seitenleiste.
   - Geben Sie `Remote - SSH` in die Suchleiste ein und installieren Sie die von Microsoft entwickelte Erweiterung.

## Schritt 2: Verbindung zum Server über SSH (Vom lokalen Computer)

1. **Über SSH in Visual Studio Code verbinden:**

   - Drücken Sie `Ctrl+Shift+P` in Visual Studio Code und wählen Sie `Remote-SSH: Connect to Host...`.
   - Geben Sie Ihre Serveradresse im Format `ssh root@ip_adresse` ein.
   - Geben Sie Ihr Passwort oder Ihren SSH-Schlüssel ein, um die Verbindung abzuschließen.

   **Hinweis:** Ersetzen Sie `ip_adresse` durch die tatsächliche IP-Adresse Ihres Servers.

2. **Verbindung bestätigen:**

   - Wenn die Verbindung erfolgreich ist, erscheint der Name des verbundenen Servers in der unteren linken Ecke des Visual Studio Code-Fensters.

3. **Terminal öffnen:**
   - Nachdem Sie die Verbindung zum Server in Visual Studio Code hergestellt haben, können Sie das Terminal öffnen, indem Sie im oberen Menü auf `Terminal` > `Neues Terminal` klicken.
   - Alternativ können Sie das Terminal öffnen, indem Sie `Ctrl+` (Steuerungstaste und die Rückwärtstaste, die sich unter der ESC-Taste befindet) drücken.

## Schritt 3: Docker und Docker Compose Installation (Auf dem Server)

Nachdem die Verbindung zum Server erfolgreich hergestellt wurde, führen Sie diese Befehle auf dem Server aus.

### Docker-Installation

1. **Systempakete aktualisieren:**

   ```sh
   sudo apt update
   ```

2. **Docker installieren:**
   ```sh
   sudo apt install docker.io
   ```
3. **Überprüfen, ob der Docker-Dienst läuft:**

   ```sh
   sudo systemctl status docker
   ```

4. **Docker-Installation überprüfen:**
   ```sh
   docker --version
   ```

### Docker Compose Installation

1. **Docker Compose herunterladen:**
   ```sh
   sudo curl -L "https://github.com/docker/compose/releases/download/1.29.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
   ```
2. **Ausführungsberechtigung für Docker Compose erteilen:**

   ```sh
   sudo chmod +x /usr/local/bin/docker-compose
   ```

3. **Installation überprüfen:**
   ```sh
   docker-compose --version
   ```

## Schritt 4: zkSync Era Node starten (Auf dem Server)

1. **Git installieren (falls noch nicht installiert):**

   ```sh
   sudo apt install git
   ```

2. **Repository klonen:**

   ```sh
   git clone https://github.com/matter-labs/zksync-era.git
   ```

3. **Zu den geklonten Verzeichnissen navigieren:**

   ```sh
   cd zksync-era/docs/guides/external-node
   cd docker-compose-examples
   ```

4. **Node mit Docker Compose starten:**

   ```sh
   docker-compose -f mainnet-external-node-docker-compose.yml up -d
   ```

5. **Laufende Container überprüfen:**

   ```sh
   docker ps
   ```

   Dieser Befehl listet die derzeit laufenden Docker-Container auf. Sie sollten die Container sehen, die zum `zkSync Era` Node gehören.

6. **Die letzten 100 Logs anzeigen und folgen:**
   ```sh
   docker logs -f --tail 100 docker-compose-examples_external-node_1
   ```
   Drücken Sie `Ctrl+C`, um die Log-Ansicht zu verlassen. Dies wird Ihren Node nicht stoppen; er läuft weiterhin im Hintergrund.

## Schritt 5: Zugriff auf das Dashboard über Portweiterleitung (Auf dem lokalen Computer)

Nach dem Starten von Docker können Sie auf ein Dashboard auf Port 3000 zugreifen. Um dieses Dashboard von Ihrem lokalen Computer aus anzuzeigen, müssen Sie die Portweiterleitung in Visual Studio Code einrichten.

1. **Portweiterleitung einrichten:**

   - Drücken Sie `Ctrl+Shift+P`, um die Befehls-Palette zu öffnen.
   - Geben Sie `Forward a Port` ein und wählen Sie die Option.
   - Geben Sie `3000` als Portnummer ein und drücken Sie Enter.

   Alternativ:

   - Klicken Sie auf den Abschnitt `PORTS` in der unteren rechten Ecke von Visual Studio Code.
   - Wählen Sie im Menü `Forward Port...`.
   - Geben Sie `3000` als Portnummer ein und drücken Sie Enter.

2. **Zugriff auf das Dashboard:**

   - Öffnen Sie Ihren Webbrowser und gehen Sie zu `http://localhost:3000/d/0/external-node`.
   - Sie sollten jetzt Zugriff auf das Dashboard haben.

Dieser Schritt ermöglicht es Ihnen, das Dashboard der auf Ihrem Remote-Server laufenden Anwendung einfach von Ihrem lokalen Computer aus anzuzeigen.

## Zusätzliche Informationen

- **Container-Logs anzeigen:**

  ```sh
  docker logs <container_id>
  ```

- **Node stoppen:**
  ```sh
  docker-compose -f mainnet-external-node-docker-compose.yml down
  ```
