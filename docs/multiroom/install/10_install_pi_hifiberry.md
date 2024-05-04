# Serveur principal

## Rôle

Ce serveur servira 

* de serveur Snapcast pour le multiroom
* de serveur Mopidy pour Spotify ou des fichiers locaux typiquement
* de récepteur bluetooth

## Prérequis

* Rasperry PI 3B+
* Raspberry Pi OS : Bookworm Lite
* Hat Hifiberry Digi+

## Packages

``` sh
sudo apt install bluez-alsa-utils
```

Nécessaire pour l'audio BlueTooth

## Configuration système

``` config title="/boot/firmware/config.txt"
#dtparam=audio=on
dtoverlay=vc4-kms-v3d,noaudio
dtoverlay=hifiberry-digi
```

Inutile de désactiver l'audio via HDMI. IL peut être utile.

## Installation Snapserver & Snapclient

``` sh
sudo apt install snapserver
wget https://github.com/badaix/snapweb/releases/download/v0.6.0/snapweb_0.6.0-1_all.deb
dpkg -i snapweb_0.6.0-1_all.deb
sudo apt install snapclient
mkdir /run/snapserver
chown _snapserver:audio /run/snapserver
chmod 0775 /run/snapserver*
systemctl restart snapserver
```

## Fichiers de configuration

### Création du fichier ```/etc/asound.conf```

Ce fichier permet de créer une "sortie" audio bleutooth sous forme d'un [fichier FIFO ](https://man7.org/linux/man-pages/man7/fifo.7.html).

```config title="/etc/asound.conf"
pcm.bluetooth {
    type plug
    slave.pcm rate48000Hz
}

pcm.rate48000Hz {
    type rate
    slave {
        pcm writeFile # Direct to the plugin which will write to a file
        format S16_LE
        rate 48000
    }
}

pcm.writeFile {
    type file
    slave.pcm null
    file "/run/bluefifo"
    format "raw"
}
```

### Mise à jour du fichier ```/etc/snapserver.conf```

Dans se fichier, les sources redirigée vers snapserver sont définies dans la section ```stream```.
De plus, le chemin racine de l'interface Web de snapserver peut être configurée.

```config title="/etc/snapserver.conf"
[stream]
...
source = pipe:///run/snapserver/mopidyfifo?name=Mopidy
source = pipe:///run/bluefifo?name=Bluetooth

[http]
...
doc_root = /usr/share/snapweb
...

```


## Installation MOPIDY

### Installation du serveur Mopidy

``` sh
sudo mkdir -p /etc/apt/keyrings
sudo wget -q -O /etc/apt/keyrings/mopidy-archive-keyring.gpg https://apt.mopidy.com/mopidy.gpg
sudo wget -q -O /etc/apt/sources.list.d/mopidy.list https://apt.mopidy.com/bullseye.list
sudo apt update
sudo apt install --no-install-recommends mopidy 
sudo apt install --no-install-recommends python3-pip
sudo pip install --break-system-packages mopidy-iris
```

!!! warning
    La documentation du plugin sur [Mopidy-Spotify](https://mopidy.com/ext/spotify/) est incorrecte. Il faut aller voir la [doc sur Github](https://github.com/mopidy/mopidy-spotify) et forcer l'installation de la version 5.0.0a1 en date du 24/03/2024.

### Installation du plugin Spotify

``` sh
sudo pip install --break-system-packages Mopidy-Spotify==5.0.0a1
wget https://github.com/kingosticks/gst-plugins-rs-build/releases/download/gst-plugin-spotify_0.12.2-1/gst-plugin-spotify_0.12.2-1_armhf.deb
sudo apt install ./gst-plugin-spotify_0.12.2-1_armhf.deb
```

Il faut récupérer le client id / client secret pour l'utilisation des API Spotify.

!!! warning
    Il faut un compte Spotify Premium

Quelques éléments restent à configurer dans le fichier de configuration Mopidy.

``` config title="/etc/mopidy/mopidy.conf - Ajout des lignes"
[http]
hostname = 0.0.0.0

[spotify]
username=<username>
password=<password>
client_id = <clientid>
client_secret = <client secret>

[audio]
output = audioresample ! audioconvert ! audio/x-raw,rate=48000,channels=2,format=S16LE ! filesink location=/run/snapserver/mopidyfifo
```

La section ```audio``` permet de définir une nouvelle "sortie" audio sous la forme d'un nouveau fichier FIFO qui sera lu par Snapserver

## Références

* [NVM](https://github.com/nvm-sh/nvm?tab=readme-ov-file#installing-and-updating)
* [Configuration Hifiberry Linux](https://www.hifiberry.com/docs/software/configuring-linux-3-18-x/)
* [Redirect A2DP sink](https://discourse.osmc.tv/t/redirect-a2dp-sink/89735/3)
* [fifo permission issues 1](https://github.com/badaix/snapcast/issues/737)
* [fifo permission issues 2](https://github.com/badaix/snapcast/issues/486)