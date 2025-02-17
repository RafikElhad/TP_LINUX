Vagrant.configure("2") do |config|
  # Définir l'image de base
  config.vm.box = "ubuntu/bionic64"  # Ubuntu 18.04 LTS

  # Donner un nom à la machine virtuelle
  config.vm.define "mini-app-front"

  # Configuration du réseau 
  config.vm.network "forwarded_port", guest: 80, host: 8080  # Nginx
  config.vm.network "forwarded_port", guest: 3306, host: 13306  # MySQL

  # Augmenter le délai d'attente pour le démarrage de la VM
  config.vm.boot_timeout = 300  # Timeout de 300 secondes (5 minutes)

  # Provisioning
  config.vm.provision "shell", inline: <<-SHELL
    # Mettre à jour le système
    sudo apt-get update -y
    sudo apt-get upgrade -y

    # Installer Nginx
    sudo apt-get install nginx -y

    # Démarrer et activer Nginx
    sudo systemctl start nginx
    sudo systemctl enable nginx

    # Installer MySQL
    sudo apt-get install mysql-server -y

    # Définir un mot de passe pour MySQL root
    sudo mysql -e "ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'comores@269';"

    # Créer un répertoire pour votre application frontale
    sudo mkdir -p /var/www/html/myapp
    
    # Déployer votre mini application frontale
    echo "<html><h1>Bienvenue sur mon application frontale!</h1></html>" | sudo tee /var/www/html/myapp/index.html

    # Configurer Nginx pour servir l'application frontale
    echo "server {
        listen 80;
        server_name localhost;

        location / {
            root /var/www/html/myapp;
            index index.html;
        }

        location /favicon.ico {
            # Gérer les requêtes pour le favicon
            access_log off; log_not_found off; 
        }
    }" | sudo tee /etc/nginx/sites-available/myapp

    # Activer la configuration de Nginx
    sudo ln -s /etc/nginx/sites-available/myapp /etc/nginx/sites-enabled/
    
    # Supprimer la configuration par défaut de Nginx
    sudo rm /etc/nginx/sites-enabled/default

    # Redémarrer Nginx pour appliquer les changements
    sudo systemctl restart nginx

    # Installer SSH
    sudo apt-get install openssh-server -y
  SHELL

  # Configurer le provider pour utiliser VirtualBox
  config.vm.provider "virtualbox" do |vb|
    vb.memory = "2048"  # Allouer 2 Go de RAM
    vb.cpus = 2         # Allouer 2 cœurs de CPU
  end
end