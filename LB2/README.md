# LB2
## Umgebung auf eigenem Notebook eingerichtet und funktionsfähig
### VirtualBox
![image](https://user-images.githubusercontent.com/125886145/221588621-c52ee546-af19-4caa-9dbb-9c418d112c6e.png)

### Vagrant 
<!-- ![image](https://user-images.githubusercontent.com/125886145/221589775-90819bee-0475-4036-80ad-3988c80b9c45.png)

![image](https://user-images.githubusercontent.com/125886145/221588621-c52ee546-af19-4caa-9dbb-9c418d112c6e.png) -->
![image](https://user-images.githubusercontent.com/125886145/221593839-5f7740ad-90c6-4b9a-9008-67af95aaf19d.png)


### Visualstudio-Code
![image](https://user-images.githubusercontent.com/125886145/221589209-82706b01-f92e-4504-b30f-d2bb15b6e9d1.png)

![image](https://user-images.githubusercontent.com/125886145/221589155-df07d6b7-f9db-4498-a002-28458341da63.png)

### Git-Client
![image](https://user-images.githubusercontent.com/125886145/221589041-0e8976c8-9792-4537-9c2d-896c920fd578.png)

### SSH-Key für Client  erstellt
![image](https://user-images.githubusercontent.com/125886145/221589505-e52d8849-9e13-44d3-a651-286e4e35b529.png)

<br>
<br>

## Eigene Lernumgebung ist eingerichtet
### GitHub oder Gitlab-Account erstellt
Für das Repository in welchem Sie sich gerade befinden, brauchte ich einen Account, also ja.

### Git-Client wurde verwendet

### Dokumentation ist als Mark Down vorhanden
Da Sie das hier lesen, ist meine Dokumentation als Mark Down vorhanden.

### Mark down-Editor ausgewählt und eingerichtet
Ohne diesen könnten Sie meine Dokumentation hier nicht lesen, also habe ich diesen verwendet.

### Markdown ist strukturiert

### Persönlicher Wissensstand dokumentiert

### Wichtigste Lernschritte sind dokumentiert

<br>
<br>

## Vagrant

### Netzwerkplan
<br>
![image](https://user-images.githubusercontent.com/125886145/224711057-ec366881-4d00-47b2-ba51-7002392f6a6b.png)
<br>

### Bestehende cm aus Vagrant-Cloud eingerichtet
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



### Eingerichtet Umgebung ist dokumentiert
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

### Funktionsweise getestet

### andere, vorfefertigte cm auf eigenem Notebook aufgesetzt

### Projekt mit Git und Markdown dokumentiert

<br>
<br>

## Sicherheitsaspekte sind implementiert
### Firewall eingerichtet inkl. Rules

### Reverse-Proky eingerichtet

### Benutzer- und Rechtvergabe ist eingerichtet

### Zugang mit SSH-Tunnel abgesichert

### Sicherheitsmassnahmen sind dokumentiert

### Testfälle
Geht der Zugriff mit localhost:8011, so ist der Reverse Proxy erfolgreich konfiguriert.

![image](https://user-images.githubusercontent.com/125886145/224703206-37f4d4fb-0d38-4f97-a0b7-fdc0dab19ced.png)
Curl proof.

![image](https://user-images.githubusercontent.com/125886145/224705192-a2a9fb97-6c33-43f8-a296-43fac6cd43e2.png)
Alle Benutzer sind erfolgreich und korrekt eingerichtet. Dies kann man am File in /etc/passwd sehen.

Um den Zugriff auf die Webseite zu erlangen, ist der Port 80 auf 8011 weitergeleitet.

### Projekt mit Git und Mark Down dokumentiert
![image](https://user-images.githubusercontent.com/125886145/223121317-29f7e78a-3862-4c36-a0b2-55c8ed210ff7.png)

![image](https://user-images.githubusercontent.com/125886145/223127282-733c59d2-8501-4624-9401-62e7eb994f6b.png)



![image](https://user-images.githubusercontent.com/125886145/223131969-60fb59c3-5688-4575-b136-4119ac78e180.png)



![image](https://user-images.githubusercontent.com/125886145/223137480-1a4c15ec-11ff-492c-9d05-fd76f79633fe.png)
Nun geht es. Ich hatte vorhin Port 101 für den Host verwendet, aber mit diesem ging es nicht. Also habe ich den Port zu 8011 gewechselt da dieser sicher frei ist. Nun geht es endlich
![224703688-83d051b9-40ec-4df1-8891-4fd0eb801e12](https://user-images.githubusercontent.com/125886145/224703792-86e986ad-d445-47ac-8703-71b016c5be8c.png)



<br>
<br>

## Zusätzliche Bewertungspunkte
### 

#### Kreativität

#### Komplexität

#### Umfang

### Umsetzung eigener Ideen
#### Cloud-Integration

#### Authentifizierung und Autorisierung via LDAP

#### Übungsdokumentation als Vorlage für Modul-Unterlagen erstellt

### Persönliche Lernentwicklung
#### Vergleich Vorwissen - Wissenszuwachs

#### Reflexion
#### Tag 1
An diesem Tag hatten wir zuerst viel Theorie um mal einen Überblick über das Modul bekommen. Dann bei den praktischen arbeiten gab es direkt ein Problem mit Virtualbox. Die VM startete nicht. Ich habe dann die Kerne von 1 auf 2 erhöht und dann konnte ich sie ohne weitere Probleme installieren. Auf der VM Apache zu installieren lief dann zum Glück auch ohne Probleme. Mit Vagrant gab es noch Porbleme und ich hatte keine Zeit mehr nach der Lösung zu suchen. 

#### Tag 2
An diesem Tag habe ich eine VM mit Vagrant aufgesetzt. Es gab aber ein Problem mit meinen GitHub. Ich konnte nichts mehr pushen, da ich ein push durchgeführt hatte, aber es eine Datei dadrin gab, welche die mindestgrösse überschritt. Dadurch konnte ich nicht mehr pushen da die Meldung kam, dass es wegen diesem File nicht pushen kann. Auch wenn ich das File gelöscht habe, kam immernoch die selbe Fehlermeldung. Da ich im GitHub noch nicht wirklich viel drin hatte, erstellte ich einfach ein neues Repository und richtete dieses wieder in Visual Studio Code ein. 

#### Tag 3
Heute verbrachte ich die meiste Zeit dabei ein Vagrantfile zu erstellen, womit eine VM mit Nginx erstellt werden soll. Dabei hatte ich immer wieder andere Fehler. Ein Fehler am Schluss war z.B. das es bei mir mit dem Port 100 für den Host nicht ging da er bereits gebraucht wird(Siehe Bild unten). Danach habe ich es mit dem Port 101 versucht. Die VM wurde erstellt, aber ich konnte nicht im Webbrowser darauf zugreifen. Also habe ich den Port 8011 gesetzt, da dieser sicher frei sein sollte. Danach ging es auch endlich.
![image](https://user-images.githubusercontent.com/125886145/223152475-e2946989-3135-4605-bcc6-8b00d0b98f2f.png)
