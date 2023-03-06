Vagrant.configure(2) do |config|
  config.vm.box = "ubuntu/xenial64"
  config.vm.network "forwarded_port", guest:80, host:100, auto_correct: true
  config.vm.synced_folder ".", "/var/www/html"  
config.vm.provider "virtualbox" do |vb|
  vb.memory = "512"  
end
config.vm.provision "shell", inline: <<-SHELL
	sudo apt-get update
	sudo apt-get -y install ufw
	sudo ufw --force enable
	sudo ufw allow 80/tcp
	sudo ufw allow 22
	sudo ufw allow 2222
	sudo systemctl start ssh
	sudo sed -i "s/.*PasswordAuthentication.*/PasswordAuthentication yes/g" /etc/ssh/sshd_config
	sudo sed -i "s/.*ChallengeResponseAuthentication.*/ChallengeResponseAuthentication yes/g" /etc/ssh/sshd_config
	sudo chown -c vagrant /var/mail
	sudo chmod -R 700 /var/mail
	sudo apt-get -y install nginx
	sudo unlink /etc/nginx/sites-enabled/default
	sudo touch /etc/nginx/sites-available/reverse-proxy.conf
	cat <<%EOF% | sudo tee -a /etc/nginx/sites-available/reverse-proxy.conf
	server {
       listen 80;
       listen [::]:80;
       server_name html;
       index index.html index.htm;
       root /var/www/html;
       index index.html;
       location / {
               try_files $uri $uri/ =404;
       }
	}
%EOF%
	sudo mkdir /var/www/html
	sudo touch /var/www/html/index.html
	cat <<%EOF% | sudo tee -a /var/www/html/index.html
	
	<!DOCTYPE html>
	<html>
	<head>
	<title>Welcome to the Internet</title>
	<style>
		body {
			width: 40em;
			margin: 0 auto;
			font-family: Arial, sans-serif;
		}
	</style>
	</head>
	<body>
	<h1>Willkommen!!</h1>
	<p>Wenn Sie diese Seite sehen, wurde der nginx-Webserver erfolgreich installiert.</p>
	</body>
	</html>
%EOF%
	sudo ln -s /etc/nginx/sites-available/reverse-proxy.conf /etc/nginx/sites-enabled/	
	sudo systemctl stop apache2
	sudo systemctl restart nginx
	sudo reboot
SHELL
end
