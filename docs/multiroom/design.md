# Description de la solution

## Conception générale

### Présentation

La solution générale est décrite dans la figure suivante.

```kroki-graphviz
@from_file:multiroom/figures/architecture.dot
```

### Serveur audio principal Multiroom

Ce serveur est le coeur du système. Il s'agit d'un Raspberry 3 avec une [carte DAC Hifiberry Digi+](https://www.hifiberry.com/shop/boards/hifiberry-digiplus-standard-version/). 

Il capable d'utiliser plusieurs sources dont :

* Spotify
* appareil Bluetooth
* fichiers locaux (ou réseau accessible depuis le Pi)

Les composants logiciels suivants sont installés :

* Pi OS Bookworm Lite 32 bits*
* Snapserver : serveur de diffusion multiroom
* Snapclient : client multiroom
* Mopidy : serveur audio
* Bluetooth ALSA : gestion de l'audio bluetooth

La carte DAC a une sortie audio numérique qui est connectée à un amplificateur audio HiFi.

### Client Multiroom "lite"

Il s'agit d'un Raspberry Pi Zéro W avec un [Shim DAC Audio](https://shop.pimoroni.com/products/audio-dac-shim-line-out?variant=32343184965715) Pimoroni.

Les composants logiciels suivants sont installés :

* Pi OS Bookworm Lite 32 bits*
* Snapclient : client multiroom

Il est connecté à une paire d'enceinte amplifiées au travers de la prise "line out" du DAC.

### Client Multiroom autonome

Il s'agit d'un Raspberry Pi Zéro W avec un amplificateur 35W [Raspberry Pi DigiAMP+](https://www.raspberrypi.com/products/digiamp-plus/) avec une paire d'enceinte directement connectées à la sortie de l'amplificateur DigiAmp+.

Les composants logiciels suivants sont installés :

* Pi OS Bookworm Lite 32 bits*
* Snapclient : client multiroom


## Pulse Audio vs Bleutooth ALSA

!!! abstract "From [Debian documentation](https://wiki.debian.org/BluetoothUser/a2dp)"
    In short: To connect to a given device, you need Bluetooth hardware on your PC (either built-in, or in the form of a USB dongle), the Bluez daemon, and a compatible audio server (either PulseAudio or PipeWire). Alternatively Bluetooth ALSA available since Debian 12 bookworm allows to avoid running of a high-level sound server.

La solution proposée ici s'appuie sur Bluetooth Alsa.