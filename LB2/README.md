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


Port forwarding von 80 auf 100.
    
    `config.vm.network "forwarded_port", guest:80, host:100, auto_correct: false`


Bestimmen mit welchem Programm die VM erstellt werden soll
    
    `config.vm.provider "virtualbox" do |vb|`


Festlegen wieviel Arbeitsspeicher verwendet werden darf
    
    `vb.memory = "1024"`

Ende der Vagrant Config
    
    `end`


VM Config einleiten. Festlegen das folgende zeilen in der Shell geschrieben werden.

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


Firewall Rules setzten
    
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

### Projekt mit Git und Mark Down dokumentiert
![image](https://user-images.githubusercontent.com/125886145/223121317-29f7e78a-3862-4c36-a0b2-55c8ed210ff7.png)

![image](https://user-images.githubusercontent.com/125886145/223127282-733c59d2-8501-4624-9401-62e7eb994f6b.png)



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
