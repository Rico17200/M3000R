# LB2

## Inhaltsverzeichnis
- [LB2](#lb2)
  - [Inhaltsverzeichnis](#inhaltsverzeichnis)
    - [Netzwerkplan](#netzwerkplan)
    - [Bestehende vm aus Vagrant-Cloud eingerichtet](#bestehende-vm-aus-vagrant-cloud-eingerichtet)
    - [Kennt die Vagrant-Befehle](#kennt-die-vagrant-befehle)
    - [VM-Webserver erstellen](#vm-webserver-erstellen)
    - [VM-Server mit UFW Firewall Config](#vm-server-mit-ufw-firewall-config)
    - [VM-Server mit Secure Shell Server](#vm-server-mit-secure-shell-server)
    - [VM-Server mit Secure Shell Server, Reverse Proxy, Benutzerberechtigungen und Firewallregeln](#vm-server-mit-secure-shell-server-reverse-proxy-benutzerberechtigungen-und-firewallregeln)
    - [Eingerichtet Umgebung Dokumentation vom Code](#eingerichtet-umgebung-dokumentation-vom-code)
  - [Testfälle](#testfälle)
    - [Zugriff funktioniert](#zugriff-funktioniert)
    - [Firewall eingerichtet inkl. Rules](#firewall-eingerichtet-inkl-rules)
    - [Benutzer- und Rechtvergabe ist eingerichtet](#benutzer--und-rechtvergabe-ist-eingerichtet)
    - [Zugang mit SSH-Tunnel abgesichert](#zugang-mit-ssh-tunnel-abgesichert)
    - [Reverse-Proky eingerichtet](#reverse-proky-eingerichtet)
    - [Persönliche Lernentwicklung](#persönliche-lernentwicklung)
      - [Vergleich Vorwissen - Wissenszuwachs](#vergleich-vorwissen---wissenszuwachs)
      - [Vorwissen im Bezug zum Modul](#vorwissen-im-bezug-zum-modul)
      - [Neues Wissen](#neues-wissen)
      - [Reflexion](#reflexion)
      - [Tag 1](#tag-1)
      - [Tag 2](#tag-2)
      - [Tag 3](#tag-3)
      - [Tag 4](#tag-4)

<br>
<br>

### Netzwerkplan
<br>

![Netzwerkplan_Rico](https://user-images.githubusercontent.com/125886145/224711821-10bad918-ab3e-42ca-b12b-09d9482f08f5.png)

<br>

### Bestehende vm aus Vagrant-Cloud eingerichtet
```Shell
      $ vagrant init ubuntu/xenial64        #Vagrantfile erzeugen
      $ vagrant up --provider virtualbox    #Virtuelle Maschine erstellen & starten
``` 

### Kennt die Vagrant-Befehle

Die wichtigsten Befehle sind:

| Befehl                    | Beschreibung                                                      |
| ------------------------- | ----------------------------------------------------------------- | 
| `vagrant init`            | Initialisiert im aktuellen Verzeichnis eine Vagrant-Umgebung und erstellt, falls nicht vorhanden, ein Vagrantfile |
| `vagrant up`              |  Erzeugt und Konfiguriert eine neue Virtuelle Maschine, basierend auf dem Vagrantfile |
| `vagrant ssh`             | Baut eine SSH-Verbindung zur gewünschten VM auf                   |
| `vagrant status`          | Zeigt den aktuellen Status der VM an                              |
| `vagrant port`            | Zeigt die Weitergeleiteten Ports der VM an                        |
| `vagrant halt`            | Stoppt die laufende Virtuelle Maschine                            |
| `vagrant destroy`         | Stoppt die Virtuelle Maschine und zerstört sie.                   |

Weitere Befehle unter: https://www.vagrantup.com/docs/cli/

<br>

### VM-Webserver erstellen 

Zuerst ordner anlegen in der die VM sein soll und eine Datei "Vagrantfile" erstellen (ohne Dateiendung).
In das Vagrant folgenden inhalt schreiben: <br>
```
Vagrant.configure(2) do |config|
    config.vm.box = "ubuntu/xenial64"
    config.vm.network "forwarded_port", guest:80, host:100, auto_correct: true
    config.vm.synced_folder ".", "/var/www/html"  
  config.vm.provider "virtualbox" do |vb|
    vb.memory = "512"  
  end
  config.vm.provision "shell", inline: <<-SHELL
    # Packages vom lokalen Server holen
    # sudo sed -i -e"1i deb {{config.server}}/apt-mirror/mirror/archive.ubuntu.com/ubuntu xenial main restricted" /etc/apt/sources.list 
    sudo apt-get update
    sudo apt-get -y install apache2 
  SHELL
  end
```
Nun muss man nur noch in einer Shell in das Verzeichnis gehen und "`vagrant up`" eintippen.

### VM-Server mit UFW Firewall Config

Bei dieser VM wird gleich die Firewall mit installiert und zusätzlich noch rules erstellt.
```
Vagrant.configure(2) do |config|
    config.vm.box = "ubuntu/xenial64"
    config.vm.network "forwarded_port", guest:80, host:100, auto_correct: true
    config.vm.synced_folder ".", "/var/www/html"  
  config.vm.provider "virtualbox" do |vb|
    vb.memory = "512"  
  end
  config.vm.provision "shell", inline: <<-SHELL
    # Packages vom lokalen Server holen
    # sudo sed -i -e"1i deb {{config.server}}/apt-mirror/mirror/archive.ubuntu.com/ubuntu xenial main restricted" /etc/apt/sources.list 
    sudo apt-get install ufw
    sudo ufw enable
    sudo ufw allow 80/tcp
    sudo ufw allow from 192.168.1.122 to any port 22
    sudo ufw allow from 192.168.1.124 to any port 3306
  SHELL
  end
```

### VM-Server mit Secure Shell Server

Bei dieser VM wird ein Secure Shell Server mit zusätzlichen Rules erstellt.
```
Vagrant.configure(2) do |config|
  config.vm.box = "ubuntu/xenial64"
  config.vm.network "forwarded_port", guest:80, host:8080, auto_correct: true
  config.vm.synced_folder ".", "/var/www/html"  
config.vm.provider "virtualbox" do |vb|
  vb.memory = "512"  
end
config.vm.provision "shell", inline: <<-SHELL
	sudo ufw --force enable
	sudo ufw allow 22
	sudo ufw allow 2222
	sudo systemctl start ssh
	sudo sed -i "s/.*PasswordAuthentication.*/PasswordAuthentication yes/g" /etc/ssh/sshd_config
	sudo sed -i "s/.*ChallengeResponseAuthentication.*/ChallengeResponseAuthentication yes/g" /etc/ssh/sshd_config
	sudo reboot
SHELL
end
```

### VM-Server mit Secure Shell Server, Reverse Proxy, Benutzerberechtigungen und Firewallregeln

Bei dieser VM wird ein Secure Shell Server installiert, Reverse Proxy, Benutzerberechtigungen und Firewallregeln eingerichtet.
```
Vagrant.configure(2) do |config|
  config.vm.box = "ubuntu/xenial64"
  config.vm.network "forwarded_port", guest:80, host:8080, auto_correct: true
  config.vm.synced_folder ".", "/var/www/html"  
config.vm.provider "virtualbox" do |vb|
  vb.memory = "512"  
end
config.vm.provision "shell", inline: <<-SHELL
	#VM Update
	sudo apt-get update
	#Apache Webserver installieren
	sudo apt-get -y install apache2
	#Firewall "enablen"
	sudo ufw --force enable
	#Port 22 und 2222 erlaunben/freischalten
	sudo ufw allow 22
	sudo ufw allow 2222
	#SSH-Service starten
	sudo systemctl start ssh
	#Im sshd_config File zwei "Statments" mit Ja austauschen, damit keine Fehlermeldung kommnt
	sudo sed -i "s/.*PasswordAuthentication.*/PasswordAuthentication yes/g" /etc/ssh/sshd_config
	sudo sed -i "s/.*ChallengeResponseAuthentication.*/ChallengeResponseAuthentication yes/g" /etc/ssh/sshd_config
	#Owner von edm Verzeichis /var/mail an den User vagrant geben
	sudo chown -c vagrant /var/mail
	#Nur noch den Besitzer (in diesem Fall vagrant) auf das Verzeichnis erlauben
	sudo chmod -R 700 /var/mail
	#Nginx --> Reverse-proxy installieren
	sudo apt-get -y install nginx
	#Virtual Host deaktivieren
	sudo unlink /etc/nginx/sites-enabled/default
	#File für Reverse-proxy erstellen
	sudo touch /etc/nginx/sites-available/reverse-proxy.conf
	#Inhalt in File einfügen
	cat <<%EOF% | sudo tee -a /etc/nginx/sites-available/reverse-proxy.conf
	server {
		listen 8080;
		location / {
			proxy_pass http://127.0.0.1;
		}
	}
%EOF%
	sudo ln -s /etc/nginx/sites-available/reverse-proxy.conf /etc/nginx/sites-enabled/reverse-proxy.conf
	#Apache Service stoppen
	sudo systemctl stop apache2
	#Nginx Service starten
	sudo systemctl restart nginx
	#VM rebooten
	sudo reboot
SHELL
end
```


<br><br>
### Eingerichtet Umgebung Dokumentation vom Code
Hier sieht man den verwendeten Code und die Erklärung zu den jeweiligen Commands.

Vagrant konfiguration einleiten

    `Vagrant.configure(2) do |config|`


Welche Ubuntu Version verwendet wird
    
    `config.vm.box = "ubuntu/bionic64"`


Port forwarding von 80 auf 8011.
    
    `config.vm.network "forwarded_port", guest:80, host:8011, auto_correct: false`


Bestimmen mit welchem Programm die VM erstellt werden soll
    
    `config.vm.provider "virtualbox" do |vb|`


Festlegen wieviel Arbeitsspeicher verwendet werden darf
    
    `vb.memory = "1024"`

Ende der Vagrant Config
    
    `end`


VM Config einleiten. Festlegen das folgende Zeilen in der Shell geschrieben werden.

    `config.vm.provision "shell", inline: <<-SHELL`


Neueste Updates herunterladen

    `sudo apt-get update`


Installieren von diversen Diensten
    
    `sudo apt-get install -y \`
    `ca-certificates \`
    `curl \`
    `gnupg \`
    `ufw \`
    `lsb-release`


Verifikation des Dockerimages

    `curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor `-o /usr/share/keyrings/docker-archive-keyring.gpg`
    `echo \`
    `"deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/`docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu \`
    `$(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > ``/dev/null`


Firewall Rules setzen
    
    `sudo ufw --force enable`
    `sudo ufw allow 80/tcp`
    `sudo ufw allow 22`
    `sudo ufw allow 2222`


Install Docker

    `sudo apt-get install docker-ce docker-ce-cli containerd.io -y`
    
    
Volume für nginx erstellen und Nginx aktivieren und Port festlegen    
    
    `docker volume create nginx_data`
    `docker run --name some-nginx -d -p 80:80 nginx`


Vagrant Config Ende
    
    `SHELL`
    `end`

<br><br>

## Testfälle

### Zugriff funktioniert

Zuerst konnte ich die VM mit dem Vagrantfile nicht erstellen.

![image](https://user-images.githubusercontent.com/125886145/223131969-60fb59c3-5688-4575-b136-4119ac78e180.png)

<br>
Nun geht es. Ich hatte vorhin Port 101 für den Host verwendet, aber mit diesem ging es nicht. Also habe ich den Port zu 8011 gewechselt da dieser sicher frei ist. Nun geht es endlich
<br>

![image](https://user-images.githubusercontent.com/125886145/223137480-1a4c15ec-11ff-492c-9d05-fd76f79633fe.png)

<br>


### Firewall eingerichtet inkl. Rules



### Benutzer- und Rechtvergabe ist eingerichtet

### Zugang mit SSH-Tunnel abgesichert

### Reverse-Proky eingerichtet
Geht der Zugriff mit localhost:8011, so ist der Reverse Proxy erfolgreich konfiguriert. Dies konnte mit Curl getestet werden.

![image](https://user-images.githubusercontent.com/125886145/224703206-37f4d4fb-0d38-4f97-a0b7-fdc0dab19ced.png)
<br>

![image](https://user-images.githubusercontent.com/125886145/224705192-a2a9fb97-6c33-43f8-a296-43fac6cd43e2.png)
<br>
Alle Benutzer sind erfolgreich und korrekt eingerichtet. Dies kann man am File in /etc/passwd sehen.

Um den Zugriff auf die Webseite zu erlangen, ist der Port 80 auf 8011 weitergeleitet.


![image](https://user-images.githubusercontent.com/125886145/223121317-29f7e78a-3862-4c36-a0b2-55c8ed210ff7.png)

![image](https://user-images.githubusercontent.com/125886145/223127282-733c59d2-8501-4624-9401-62e7eb994f6b.png)





![image](https://user-images.githubusercontent.com/125886145/223127282-733c59d2-8501-4624-9401-62e7eb994f6b.png)




![224703688-83d051b9-40ec-4df1-8891-4fd0eb801e12](https://user-images.githubusercontent.com/125886145/224703792-86e986ad-d445-47ac-8703-71b016c5be8c.png)



<br>
<br>


### Persönliche Lernentwicklung
#### Vergleich Vorwissen - Wissenszuwachs

#### Vorwissen im Bezug zum Modul
- Basic Linux Kenntnise
- Schonmal von Container gehört aber nie verwendet

#### Neues Wissen
- Ich lernte Vagrant kennen
- Ich lernte mit GitHub zu arbeiten, da ich noch nie damit arbeitete
- Ich erfuhr wie man den Prozess vom aufsetzen automatisieren kann
- Mir wurden die Vorteile davon klar
- Ich lernte verschiedene Sicherheitsaspekte kennen
  - Die Begriffe Authentizität und Authentisierung wurden aufgefrischt, da ich schon länger nicht mehr damit zu tun hatte
- Ich lernte wie man die Firewall auf Linux konfiguriert
- Die Benutzung von Proxys wurden mir wieder bewusst, da ich vor langer Zeit schonmal damit gearbeitet habe

#### Reflexion
#### Tag 1
An diesem Tag hatten wir zuerst viel Theorie um mal einen Überblick über das Modul bekommen. Dann bei den praktischen arbeiten gab es direkt ein Problem mit Virtualbox. Die VM startete nicht. Ich habe dann die Kerne von 1 auf 2 erhöht und dann konnte ich sie ohne weitere Probleme installieren. Auf der VM Apache zu installieren lief dann zum Glück auch ohne Probleme. Mit Vagrant gab es noch Porbleme und ich hatte keine Zeit mehr nach der Lösung zu suchen. 

#### Tag 2
An diesem Tag habe ich eine VM mit Vagrant aufgesetzt. Es gab aber ein Problem mit meinen GitHub. Ich konnte nichts mehr pushen, da ich ein push durchgeführt hatte, aber es eine Datei dadrin gab, welche die mindestgrösse überschritt. Dadurch konnte ich nicht mehr pushen da die Meldung kam, dass es wegen diesem File nicht pushen kann. Auch wenn ich das File gelöscht habe, kam immernoch die selbe Fehlermeldung. Da ich im GitHub noch nicht wirklich viel drin hatte, erstellte ich einfach ein neues Repository und richtete dieses wieder in Visual Studio Code ein. 

#### Tag 3
Heute verbrachte ich die meiste Zeit dabei ein Vagrantfile zu erstellen, womit eine VM mit Nginx erstellt werden soll. Dabei hatte ich immer wieder andere Fehler. Ein Fehler am Schluss war z.B. das es bei mir mit dem Port 100 für den Host nicht ging da er bereits gebraucht wird(Siehe Bild unten). Danach habe ich es mit dem Port 101 versucht. Die VM wurde erstellt, aber ich konnte nicht im Webbrowser darauf zugreifen. Also habe ich den Port 8011 gesetzt, da dieser sicher frei sein sollte. Danach ging es auch endlich.
![image](https://user-images.githubusercontent.com/125886145/223152475-e2946989-3135-4605-bcc6-8b00d0b98f2f.png)

#### Tag 4
Am Anfang musste ich meine Vagrant VM wieder zum laufen bekommen, damit ich weiter arbeiten kann. Dies ging aber ganz leicht mit dem Befehl "vagrant up". Danach funktioniert alles wieder wie letztes mal. Ich konnte dann noch Testfälle dazu erstellen und testen. Es lief dabei alles ohne Probleme durch, was sehr erfreulich ist. Danach habe ich auch noch meine Dokumentation angepasst. z.B. habe ich die Punkte aus dem Bewertungsraster unter "Umgebung auf eigenem Notebook eingerichtet und funktionsfähig" aus meiner Dokumentation gelöscht, da diese die Voraussetzung für die spätere Arbeit sind und diese habe ich dann wiederum gut dokummentiert. 
Abschliessend kann man sagen das heute der einzige Tag war, an dem keine wirkichen Probleme auftraten, was überraschend war, aber ich natürlich sehr gerne so nehme.
