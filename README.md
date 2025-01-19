# nginxproxymanagerTutoriel
Nginx Proxy Manager - Guide d'installation et de configuration
Dans ce guide, nous allons voir comment installer et configurer Nginx Proxy Manager à l'aide de Docker.

Nous utiliserons le logiciel libre et open-source Nginx Proxy Manager.

Page officielle du projet nginx : https://nginxproxymanager.com/
Documentation : [Guide officiel](https://nginxproxymanager.com/guide/)

Prérequis :
Un serveur Linux avec Ubuntu 20.04 LTS (ou une version plus récente).
Un nom de domaine pointant vers l’adresse IP publique de votre serveur Linux.
Docker peut être installé sur un autre système Linux, mais cela nécessitera des commandes spécifiques selon le système.
Étapes d'installation :
1. Installer Docker et Docker-Compose
1.1 Installer Docker

sudo apt update  
sudo apt install apt-transport-https ca-certificates curl gnupg-agent software-properties-common  
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -  
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"  
sudo apt update  
sudo apt-get install docker-ce docker-ce-cli containerd.io  
1.2 Vérifier si Docker est correctement installé

sudo docker run hello-world  
1.3 Installer Docker-Compose
Téléchargez la dernière version de Docker-Compose :


sudo curl -L "https://github.com/docker/compose/releases/download/1.25.5/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose  
sudo chmod +x /usr/local/bin/docker-compose  

1.4 Vérifier si Docker-Compose est correctement installé
sudo docker-compose --version  

1.5 (Optionnel) Ajouter votre utilisateur Linux au groupe Docker

sudo usermod -aG docker $USER  
2. Configurer Nginx Proxy Manager
2.1 Créer un fichier docker-compose.yml
Utilisez le modèle suivant pour votre fichier :

yaml
version: '3.5'  
services:  
  app:  
    image: 'jc21/nginx-proxy-manager:latest'  
    ports:  
      - '80:80'  
      - '81:81'  
      - '443:443'  
    environment:  
      DB_MYSQL_HOST: "db"  
      DB_MYSQL_PORT: 3306  
      DB_MYSQL_USER: "npm"  
      DB_MYSQL_PASSWORD: "npm"  
      DB_MYSQL_NAME: "npm"  
    volumes:  
      - ./data:/data  
      - ./letsencrypt:/etc/letsencrypt  
  db:  
    image: 'jc21/mariadb-aria:latest'  
    environment:  
      MYSQL_ROOT_PASSWORD: 'npm'  
      MYSQL_DATABASE: 'npm'  
      MYSQL_USER: 'npm'  
      MYSQL_PASSWORD: 'npm'  
    volumes:  
      - ./mysql:/var/lib/mysql 
      
2.2 Démarrer Nginx Proxy Manager
docker-compose up -d  

3. Se connecter à l'interface web de Nginx Proxy Manager
Ouvrez votre navigateur et connectez-vous à votre serveur via son adresse IP ou son nom de domaine (FQDN) sur le port 81.
Identifiez-vous avec :
Utilisateur : admin@example.com
Mot de passe : changeme
Changez immédiatement ces identifiants pour sécuriser votre installation.
