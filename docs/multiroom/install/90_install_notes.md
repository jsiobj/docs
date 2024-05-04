# Notes d'installation

## Impossible de démarrer la version 0.27 sur Debian Bookworms

``` shell
wget https://github.com/badaix/snapcast/releases/download/v0.27.0/snapclient_0.27.0-1_without-pulse_armhf.deb
wget https://github.com/badaix/snapcast/releases/download/v0.27.0/snapserver_0.27.0-1_armhf.deb
wget http://ftp.debian.org/debian/pool/main/f/flac/libflac8_1.3.3-2+deb11u2_armhf.deb
sudo dpkg -i libflac8_1.3.3-2+deb11u2_armhf.deb
sudo dpkg -i snapserver_0.27.0-1_armhf.deb
apt install
sudo dpkg -i snapclient_0.27.0-1_without-pulse_armhf.deb
apt install
```

Mais impossible de démarrer snapclient

[Unable to install the provided deb package on bookworm-based raspbian](https://github.com/badaix/snapcast/issues/1163)

Apparement, le prolème est résolu.

## Utilisateur Snapcast

Les utilisateurs ```snapserver``` et ```snapclient``` ont été renommés ```\_snapserver``` et ```\_snapclient``` en version 0.16 puis re-renommés en ```snapserver``` et ```snapclient``` en 0.17. Mais sur la version 0.26 (la 0.27 ne fonctionne pas sur Bookworm), les utilisateurs sont toujours préfixés avec ```_```.
