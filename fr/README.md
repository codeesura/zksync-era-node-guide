# Guide de Configuration du Nœud zkSync Era

Ce guide explique étape par étape comment configurer un nœud `zkSync Era` sur Ubuntu Linux.

## Exigences Système

- CPU: Un processeur relativement moderne est recommandé.
- RAM: 32 Go
- Stockage: 300 Go, avec une croissance de l'état d'environ 1 To par mois.
- Réseau: Connexion de 100 Mbps (1 Gbps+ recommandé)

### Location de Serveur

Si vous avez besoin d'un serveur, vous pouvez en louer un chez Hetzner. De plus, si vous vous inscrivez en utilisant ce lien de parrainage, vous recevrez un crédit de 20 EUR.

[Hetzner](https://hetzner.cloud/?ref=fu2umOyLCWhh)

## Étape 1: Installation de Visual Studio Code (Sur l'Ordinateur Local)

1. **Télécharger et Installer Visual Studio Code:**
   [Page de Téléchargement de Visual Studio Code](https://code.visualstudio.com/)

   - Une fois téléchargé, suivez l'assistant d'installation pour installer Visual Studio Code.

2. **Installer l'Extension SSH:**
   - Ouvrez Visual Studio Code.
   - Cliquez sur l'icône des `Extensions` dans la barre latérale gauche.
   - Tapez `Remote - SSH` dans la barre de recherche et installez l'extension développée par Microsoft.

## Étape 2: Connexion au Serveur via SSH (Depuis l'Ordinateur Local)

1. **Se Connecter à SSH dans Visual Studio Code:**

   - Appuyez sur `Ctrl+Shift+P` dans Visual Studio Code et sélectionnez `Remote-SSH: Connect to Host...`.
   - Entrez l'adresse de votre serveur au format `ssh root@adresse_ip`.
   - Entrez votre mot de passe ou votre clé SSH pour compléter la connexion.

   **Remarque:** Remplacez `adresse_ip` par l'adresse IP réelle de votre serveur.

2. **Vérification de la Connexion:**

   - Si la connexion est réussie, le nom du serveur connecté apparaîtra dans le coin inférieur gauche de la fenêtre de Visual Studio Code.

3. **Ouvrir le Terminal:**
   - Après avoir connecté le serveur dans Visual Studio Code, vous pouvez ouvrir le terminal en cliquant sur `Terminal` > `Nouveau Terminal` dans le menu supérieur.
   - Alternativement, vous pouvez ouvrir le terminal en appuyant sur `Ctrl+` (touche Control et la touche backtick, située sous la touche ESC).

## Étape 3: Installation de Docker et Docker Compose (Sur le Serveur)

Après vous être connecté avec succès au serveur, exécutez ces commandes sur le serveur.

### Installation de Docker

1. **Mettre à Jour les Paquets du Système:**

   ```sh
   sudo apt update
   ```

2. **Installer Docker:**

   ```sh
   sudo apt install docker.io
   ```

3. **Vérifier que le Service Docker est en Cours d'Exécution:**

   ```sh
   sudo systemctl status docker
   ```

4. **Vérifier l'Installation de Docker:**

   ```sh
   docker --version
   ```

### Installation de Docker Compose

1. **Télécharger Docker Compose:**

   ```sh
   sudo curl -L "https://github.com/docker/compose/releases/download/1.29.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
   ```

2. **Accorder la Permission d'Exécution à Docker Compose:**
   ```sh
   sudo chmod +x /usr/local/bin/docker-compose
   ```
3. **Vérifier l'Installation:**
   ```sh
   docker-compose --version
   ```

## Étape 4: Démarrer le Nœud zkSync Era (Sur le Serveur)

1. **Installer Git (si ce n'est pas déjà fait):**

   ```sh
   sudo apt install git
   ```

2. **Cloner le Répertoire:**

   ```sh
   git clone https://github.com/matter-labs/zksync-era.git
   ```

3. **Naviguer vers les Répertoires Clonés:**

   ```sh
   cd zksync-era/docs/guides/external-node
   cd docker-compose-examples
   ```

4. **Démarrer le Nœud en Utilisant Docker Compose:**

   ```sh
   docker-compose -f mainnet-external-node-docker-compose.yml up -d
   ```

5. **Vérifier les Conteneurs en Cours d'Exécution:**

   ```sh
   docker ps
   ```

   Cette commande liste les conteneurs Docker en cours d'exécution. Vous devriez voir les conteneurs liés au nœud `zkSync Era`.

6. **Afficher et Suivre les 100 Derniers Journaux:**
   ```sh
   docker logs -f --tail 100 docker-compose-examples_external-node_1
   ```

Appuyez sur `Ctrl+C` pour quitter la vue des journaux. Cela n'arrêtera pas votre nœud; il continuera à fonctionner en arrière-plan.

## Étape 5: Accéder au Tableau de Bord via le Transfert de Port (Sur l'Ordinateur Local)

Après avoir exécuté Docker, vous pouvez accéder à un tableau de bord sur le port 3000. Pour visualiser ce tableau de bord depuis votre ordinateur local, vous devez configurer le transfert de port dans Visual Studio Code.

1. **Configurer le Transfert de Port:**

   - Appuyez sur `Ctrl+Shift+P` pour ouvrir la palette de commandes.
   - Tapez `Forward a Port` et sélectionnez l'option.
   - Entrez `3000` comme numéro de port et appuyez sur Entrée.

   Alternativement:

   - Cliquez sur la section `PORTS` dans le coin inférieur droit de Visual Studio Code.
   - Sélectionnez `Forward Port...` dans le menu.
   - Entrez `3000` comme numéro de port et appuyez sur Entrée.

2. **Accéder au Tableau de Bord:**

   - Ouvrez votre navigateur web et allez à `http://localhost:3000/d/0/external-node`.
   - Vous devriez maintenant avoir accès au tableau de bord.

Cette étape vous permet de visualiser facilement le tableau de bord de l'application en cours d'exécution sur votre serveur distant depuis votre ordinateur local.

## Informations Supplémentaires

- **Afficher les Journaux des Conteneurs:**

  ```sh
  docker logs <container_id>
  ```

- **Arrêter le Nœud:**
  ```sh
  docker-compose -f mainnet-external-node-docker-compose.yml down
  ```
