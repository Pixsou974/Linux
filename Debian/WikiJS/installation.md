# INSTALLATION DE WIKI.JS SUR DEBIAN 12<br/>
## 1. Mise a jour du systeme et installation de paquets essentiels<br/>
```
sudo apt update
sudo apt upgrade -y
sudo apt install -y curl wget vim git unzip socat sudo bash-completion apt-transport-https build-essential dirmngr gnupg2 ca-certificates lsb-release debian-archive-keyring
```
## 2. Installer Node.js et npm avec NodeSource <br/>
```
# Ajouter le dépôt NodeSource pour Node.js 20.x (recommandé pour Wiki.js)
curl -fsSL https://deb.nodesource.com/setup_20.x | sudo -E bash -

# Installer Node.js et npm
sudo apt install -y nodejs
```
Pour verifier si Node.js et npm sont bien installés verifié la version en faisant ```node -v``` et ```npm -v``` pour voir apparaitre les versions<br/>
![node](https://github.com/user-attachments/assets/e440d720-47f3-4b95-8e12-88ba77742098)
## 3. Installer et configurer PostgreSQL
```
# Ajouter le dépôt PostgreSQL officiel
sudo install -d /usr/share/postgresql-common/pgdg
sudo curl -o /usr/share/postgresql-common/pgdg/apt.postgresql.org.asc --fail https://www.postgresql.org/media/keys/ACCC4CF8.asc
. /etc/os-release
sudo sh -c "echo 'deb [signed-by=/usr/share/postgresql-common/pgdg/apt.postgresql.org.asc] https://apt.postgresql.org/pub/repos/apt $VERSION_CODENAME-pgdg main' > /etc/apt/sources.list.d/pgdg.list"

# Mettre à jour les listes de paquets et installer PostgreSQL
sudo apt update
sudo apt install -y postgresql postgresql-contrib

# Vérifier l'état du service PostgreSQL
sudo systemctl status postgresql
```
## 4. Telecharger et installer Wiki.js
```
# Créer un répertoire pour Wiki.js
sudo mkdir -p /var/www/wikijs
sudo chown -R $USER:$USER /var/www/wikijs # Donne les droits à votre utilisateur courant

# Naviguer vers le répertoire
cd /var/www/wikijs

# Télécharger la dernière version de Wiki.js
wget https://github.com/Requarks/wiki/releases/latest/download/wiki-js.tar.gz

# Extraire les fichiers
tar xzf wiki-js.tar.gz

# Supprimer l'archive téléchargée
rm wiki-js.tar.gz
```
## 5. Configurer Wiki.js
```
# Copier le fichier de configuration d'exemple
mv config.sample.yml config.yml

# Ouvrir le fichier de configuration pour l'édition
nano config.yml
```
Dans le fichier ```config.yml``` modifier la section ```db``` comme ceci
```
# Configuration de la connexion a la base de données
db:
  type: postgres
  host: localhost
  port: 5432
  user: wikijs_user # Remplacez par le nom de l'utilisateur de la base de données que vous avez créé dans l'etape 3
  pass: YOUR_PASSWORD # Remplacez par le mot de passe que vous avez défini pour cet utilisateur
  db: wikijs_db # Remplacez par le nom de la base de données que vous avez créée
```
## 6. Installation de PM2 et lancement de Wiki.js
PM2 est un gestionnaire de processus pour Node.js qui maintient les applications Node.js en ligne<br/>
```
# Installer PM2 globalement
sudo npm install -g pm2

# Lancer Wiki.js avec PM2
# Assurez-vous d'être dans le répertoire /var/www/wikijs
cd /var/www/wikijs
pm2 start server --name wikijs

# Sauvegarder la configuration PM2 pour un démarrage automatique au redémarrage
pm2 save

# Configurer PM2 pour démarrer au démarrage du système
pm2 startup
```
