# Schritt 1: 
Raspberry Pi OS herunterladen von: 
https://www.raspberrypi.org/software/operating-systems/#raspberry-pi-os-32-bit

# Schritt 2: 
Balena Etcher herunterladen 
https://www.balena.io/etcher/

# Optionaler Schritt 2a: 
VS Code herunter laden :-) 
https://code.visualstudio.com/download

# Schritt 3: 
SD Karte mit mit Image flashen. 

# Schritt 4: 
ssh file auf SD Karte kopieren

# Schritt 5: 
LAN Kabel an Raspberry Pi anschließen, Raspberry Pi mit Strom versorgen.

# Schritt 6: 
Im Router nach der IP Adresse des neuen Raspberry Pi suchen.

# Schritt 7: 
Mittels ssh Befehl mit dem Raspberry Pi verbinden. 
>> ssh pi@<ipadresse>
Passwort "raspberry"

# Schritt 8: 
mit dem nächsten Befehl updaten wir den PI.
>> sudo apt-get update && sudo apt-get upgrade -y

# Schritt 9: 
als nächstes ändern wir die Grundeinstellungen. Dazu gehen wir in die Raspi-Config. 
>> sudo raspi-config

Dort ändern wir als erstes das Passwort. 
--> Punkt 1: Change User Password 
Dann die Spracheinstellungen. 
--> Punkt 4: Localisation Options, I1 Change Locale, dort dann DE-DE UTF8 mit der Leertaste markieren, dann immer weiter... 
Dann ändern wir die Zeitzone.
--> Punkt 4: Localisation Options, I2 Change Timezone, auf Berlin 
Dann ändern wir den Wifi-country.
--> Punkt 4: Localisation Options, I4 Change Wi-Fi country, und setzen das auf Germany. 
Als nächstes wechseln wir mit Back zurück und kümmern uns um die dauerhafte SSH Zugriffsmöglichkeit. 
--> Unter Punkt 5, Interfacing Options, P2 SSH setzen wir diesen auf enabled. 
Danach sorgen wir noch für mehr nutzbaren Platz auf der SD Karte. 
--> im Punkt 7 Asvanced Options, gehen wir in den Reiter A1 Expand File System.
Zu guterletzt geben wir dem Pi noch einen aussagekräftigen hostnamen. 
--> unter Punkt 2, N1 Hostname, zum Beispiel DockerPi

jetzt wird es zeit für einen Neustart. 
Mit auswählen von Finish sollte das vorgeschlagen werden, wenn nicht, dann 
>> sudo reboot


# Schritt 10: 
wieder per SSH mit dem Pi verbinden:
--> ssh pi@<ipadresse>


# Schritt 11: 
wir geben unserem pi nachdem er neu gestartet wurde eine statische IP adresse: 
>> sudo nano /etc/dhcpcd.conf
in der datei fügen wir folgendes unten an: natürlich mit für euer Netzwerk angepassten Werten...
    # Example static IP configuration:
    interface eth0
    static ip_address=192.168.2.30/24
    static routers=192.168.2.1
    static domain_name_servers=192.168.2.1


# optionaler schritt 12: 
wenn ihr am Mac seid, so wie ich, könnt ihr in der hosts datei den pi zu eurer host liste hinzufügen. 
unter /etc/hosts folgende Zeile einfügen: 
192.168.2.234 DockerPi
Um die Datei zu speichern müsst ihr root rechte haben! 


# Optionaler Schritt 13: 
um nicht immer ein Passwort eingeben zu müssen, kopieren wir die ID des pi.
dazu müsstet ihr die SSH zum pi mit 
>> exit 
trennen, dann in euer Terminal folgedes eingeben: 
>> ssh-copy-id pi@DockerPi

# Schritt 14: 
wieder mit dem Pi per SSH verbinden,
>> SSH pi@DockerPi 
falls ihr die beiden Optionalen Schritte ausgeführt habt... 
anderenfalls im lokalen terminal: 
>> ssh pi@<IPADRESSE>

# Schritt 15: 
nun geht es ans eingemachte! Wire installieren Docker: 
eigentlich benötigen wir dazu nur diese beiden befehle: 

>> curl -sSL get.docker.com | sh
sobald das durch ist, geben wir noch den user PI die docker user gruppe um nicht immer sudo vor die Befehle schreiben zu müssen:

>> sudo usermod -aG docker pi

# Schritt 16: 
jetzt installieren wir noch docker-compose. 
mit docker Compose lassen sich ganze App landschaften mit einem Enter starten:

>> sudo apt-get install libffi-dev libssl-dev
>> sudo apt-get install -y python3 python3-pip
>> sudo apt-get remove python-configparser
>> sudo pip3 install docker-compose

## Herzlichen Glückwunsch! ihr habt einen PI mit Docker :-) 
nun testen wir noch alles nachdem wir einmal neu gestartet haben: 
>> sudo reboot... 

>> ssh pi@DockerPi

>> docker info
>> docker ps 
>> docker-compose info
