Configuration de Vagrant pour mini-app-front

Ce fichier décrit la configuration utilisée dans le fichier Vagrantfile pour la machine virtuelle mini-app-front.


<img width="624" alt="V1PNG" src="https://github.com/user-attachments/assets/59e1a25a-9c85-4062-92da-d3de3ee86e7e" />



Initialisation de la configurationVagrant.configure("2") do |config|Cette ligne initialise la configuration de Vagrant. Le chiffre "2" indique la version de l'API Vagrant utilisée.

Définition de l'image de baseconfig.vm.box = "ubuntu/bionic64"Définit l'image de base pour la machine virtuelle. Ici, on utilise Ubuntu 18.04 LTS (Bionic Beaver) en version 64 bits. Cette image sera téléchargée automatiquement si elle n'est pas déjà disponible.

Définition de la machine virtuelleconfig.vm.define "mini-app-front" do |webserver|Définit une machine virtuelle nommée mini-app-front. Tout ce qui suit (indenté sous do |webserver| et end) concerne la configuration de cette machine virtuelle.

Redirection de ports (Port Forwarding)config.vm.network "forwarded_port", guest: 80, host: 8080Cette ligne configure la redirection de ports pour Nginx. Elle signifie que le port 80 de la machine virtuelle (où Nginx écoute par défaut) sera accessible depuis l'hôte (votre ordinateur) sur le port 8080. Vous pourrez donc accéder à l'application web en allant sur http://localhost:8080 dans votre navigateur.

config.vm.network "forwarded_port", guest: 3306, host: 13306Redirection de port similaire à la précédente, mais pour MySQL. Le port 3306 de la machine virtuelle (port par défaut de MySQL) est redirigé vers le port 13306 de l'hôte. Cela permet d'accéder à la base de données MySQL depuis l'ordinateur en utilisant le port 13306.

Délai de démarrageconfig.vm.boot_timeout = 300Définit un délai d'attente de 300 secondes (5 minutes) pour le démarrage de la machine virtuelle. Cela peut être utile si le processus de démarrage est long, notamment lors de l'installation de nombreux logiciels.

Provisioning (Installation automatique de logiciels)config.vm.provision "shell", inline: <<-SHELL ... SHELLCette section définit un provisioning de type shell. Le provisioning est une étape d'automatisation qui permet d'exécuter des commandes à l'intérieur de la machine virtuelle après son démarrage. Ici, un script shell est directement inclus dans le fichier Vagrant.

sudo apt-get update -yMet à jour la liste des paquets disponibles dans les dépôts APT (Advanced Package Tool). Le flag -y répond automatiquement "oui" à toutes les questions, évitant ainsi l'attente d'une interaction manuelle.

sudo apt-get upgrade -yMet à niveau les paquets installés vers leur version la plus récente. Là encore, le flag -y automatise la réponse "oui" aux questions, assurant une mise à jour complète du système.



<img width="566" alt="V2" src="https://github.com/user-attachments/assets/b469242d-c9f5-4523-8881-0d1134b86426" />



- sudo apt-get install nginx -y : Installe Nginx, un serveur web rapide et léger.
- sudo systemctl start nginx : Démarre Nginx.
- sudo systemctl enable nginx : Active Nginx au démarrage.
- sudo apt-get install mysql-server -y : Installe MySQL, un SGBD populaire.
- sudo mysql -e "ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'comores@269';" : Change le mot de passe root MySQL (exemple, à sécuriser).
- sudo mkdir -p /var/www/html/myapp : Crée le dossier de l’application web.
- echo "<html><h1>Bienvenue sur mon application frontale!</h1></html>" | sudo tee /var/www/html/myapp/index.html : Crée une page d'accueil HTML.






<img width="515" alt="V3" src="https://github.com/user-attachments/assets/d6e4b7c5-1eb8-4e02-837c-c4f8d9c06cd3" />





- echo "server { ... }" : Crée un fichier de configuration Nginx.
- listen 80; : Nginx écoute sur le port HTTP par défaut.
- server_name localhost; : Définit le nom du serveur (ici, localhost).
- location / { ... } : Gère les requêtes vers la racine du site.
- root /var/www/html/myapp; : Définit le dossier du site web.
- index index.html; : Spécifie la page d'accueil par défaut.
- location /favicon.ico { ... } : Gère les requêtes pour l’icône du site.
- access_log off; log_not_found off; : Désactive certains logs pour éviter l’encombrement.
- sudo tee /etc/nginx/sites-available/myapp : Enregistre la config du site.
- sudo ln -s /etc/nginx/sites-available/myapp /etc/nginx/sites-enabled/ : Active la config en créant un lien symbolique.



<img width="549" alt="v4" src="https://github.com/user-attachments/assets/48ec7568-8d62-43fb-80b3-e9165bb5dac8" />



- sudo rm /etc/nginx/sites-enabled/default : Supprime la config Nginx par défaut pour éviter les conflits.
- sudo systemctl restart nginx : Redémarre Nginx pour appliquer les changements.
- sudo apt-get install openssh-server -y : Installe SSH pour un accès distant sécurisé.
- SHELL : Fin du bloc de commandes shell.
- config.vm.provider "virtualbox" do |vb| ... end : Configure la VM sous VirtualBox.
- vb.memory = "2048" : Attribue 2 Go de RAM à la VM.
- vb.cpus = 2 : Alloue 2 cœurs CPU pour de meilleures performances.
- end : Termine la configuration de la VM.
- 


<img width="637" alt="vagrant up" src="https://github.com/user-attachments/assets/777b4f58-73dc-4880-870e-9c6d4581bc28" />




- vagrant validate : Vérifie que Vagrantfile est correct.
- vagrant up : Démarre la VM mini-app-front avec Ubuntu 18.04.
- Redirection des ports : Le port 80 de la VM est mappé au 8080 de l'hôte.
- Dossier partagé : D:/Cours M2GL ISI/Devops/vagrant/mini-app-front monté sur /vagrant.
- Machine prête, possibilité de reprovisionner avec vagrant provision.


<img width="506" alt="ssh" src="https://github.com/user-attachments/assets/06f0971b-1021-40e6-bbcd-a43d21f42cc0" />



La commande utilisée est vagrant ssh. Cette commande permet d'ouvrir une session SSH directement dans la machine virtuelle sans avoir besoin d'authentification manuelle.
On est connecté à une machine Ubuntu 14.04 LTS avec le noyau Linux 3.13.0-170 (64 bits).




<img width="472" alt="mysql" src="https://github.com/user-attachments/assets/bac55fb4-c6cd-49a9-8661-e297edcdd45d" />



- mysql -u root -p : Connexion à MySQL en tant que root.
- Saisie du mot de passe requise.
- Accès au moniteur MySQL confirmé (ID session, version 5.7.42, Ubuntu 18.04).
- Instructions : help ou \h pour l'aide, \c pour annuler une commande.
- Prêt pour exécuter des requêtes SQL.


<img width="359" alt="verification BD" src="https://github.com/user-attachments/assets/b8cfc68e-efe8-499d-92f8-e3dddd7769f1" />



Dans cette session MySQL, l'utilisateur effectue deux opérations principales. D'abord, 
il crée une base de données appelée devops en exécutant la commande create database devops;, 
ce qui est confirmé par le message Query OK, 1 row affected (0.00 sec), indiquant que l'opération a réussi. 
Ensuite, avec la commande show databases;, il affiche la liste des bases de données disponibles, qui inclut six entrées : information_schema, contenant des métadonnées du serveur MySQL ; devops, 
la nouvelle base créée ; mysql, utilisée pour la gestion interne, comme les utilisateurs et les privilèges ; performance_schema, dédiée à la surveillance des performances ; 
rafik, une base préexistante ajoutée auparavant ; et sys, offrant une vue simplifiée des métadonnées et des statistiques de performance. 
Le message 6 rows in set (0.00 sec) indique que six bases ont été listées et que la commande a été exécutée rapidement.



<img width="958" alt="front" src="https://github.com/user-attachments/assets/d134f30a-f480-4a14-b402-37ffd0b60276" />


on accede à notre application front deployer dans nginx





