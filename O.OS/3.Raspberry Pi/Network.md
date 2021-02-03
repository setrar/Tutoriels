# :strawberry: Static IP Change using `DHCP` Method

https://linuxconfig.org/how-to-configure-static-ip-address-on-ubuntu-20-04-focal-fossa-desktop-server


# :a: Changer l'adresse IP statique

## :zero: Verifier son environnement

:pushpin: Récuperer l'adresse de la passerelle par défaut

```
$ ip route | grep default | awk '{print $3}'
```


## :one: Modifier le fichier `/etc/netplan/50-cloud-init.yaml`

:bookmark: En enlevant les commentaires devant les lignes de configurations suivantes

:pushpin: à la maison derrière son routeur

```
# This file is generated from information provided by
# the datasource.  Changes to it will not persist across an instance.
# To disable cloud-init's network configuration capabilities, write a file
# /etc/cloud/cloud.cfg.d/99-disable-network-config.cfg with the following:
# network: {config: disabled}
network:
    ethernets:
        eth0:
            dhcp4: false
            addresses: [192.168.1.202/24]
            gateway4: 192.168.1.1
            nameservers:
              addresses: [8.8.8.8,8.8.4.4,192.168.1.1]
    version: 2
```

:pushpin: au Collège avec les adresses IP fixes `10.13.237.0/25` DNS `10.10.99.2` et `10.10.99.3`

```
# This file is generated from information provided by
# the datasource.  Changes to it will not persist across an instance.
# To disable cloud-init's network configuration capabilities, write a file
# /etc/cloud/cloud.cfg.d/99-disable-network-config.cfg with the following:
# network: {config: disabled}
network:
    ethernets:
        eth0:
            dhcp4: false
            addresses: [10.13.237.16/25]
            gateway4: 10.13.237.1
            nameservers:
              addresses: [8.8.8.8,8.8.4.4,10.10.99.2,10.10.99.3]
    version: 2
```

## :two: Redémarrer ou Éteindre la machine

:pushpin: Redémarrer

```
$ sudo reboot
```

**ou** 

:pushpin: Éteindre

```
$ sudo shutdown -h now
```

# :ab: Démarrer le service à distance `ssh`

:zero: Installation de `ssh`
 
:pushpin: installer le service

```
$ sudo systemctl enable ssh
```

:pushpin: démarrer le service

```
$ sudo systemctl start ssh
```

:one: Tester le service `ssh` 

:pushpin: localement au préalable

```
$ ssh pi@localhost
```

:pushpin: à distance

```
$ ssh pi@10.13.237.16
```

:warning: Alerte du au changement de machine donc d'adresse Ethernet

```
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
@    WARNING: REMOTE HOST IDENTIFICATION HAS CHANGED!     @
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
IT IS POSSIBLE THAT SOMEONE IS DOING SOMETHING NASTY!
Someone could be eavesdropping on you right now (man-in-the-middle attack)!
It is also possible that a host key has just been changed.
The fingerprint for the ECDSA key sent by the remote host is
SHA256:7ZfMNE9ibP4I8NbCdjoVYtaBuKzdG5Iqh4Z5mMSi/Ho.
Please contact your system administrator.
Add correct host key in /c/Users/300098957/.ssh/known_hosts to get rid of this message.
Offending ECDSA key in /c/Users/300098957/.ssh/known_hosts:13
ECDSA host key for 10.13.237.16 has changed and you have requested strict checking.
Host key verification failed.
```

Ouvrir le fichier `~/.ssh/known_hosts`et enlever la ligne ressemblant à celle ci-dessous avec l'adresse IP `10.13.237.19`
```
10.13.237.19 ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBMrWaYXRK6bd7KGr+YlDkWVB/dqYyOv6mROS/b2M0EuAq3QT4n7Dc55z4ub4c2ZN+PEqVtLmJcqcs16dcisGUV0=
```

:pushpin: à distance avec une interface UI utiliser `-Y`

```
$ ssh -Y pi@10.13.237.16
```

# References

## Raspbian

https://raspberrypi.stackexchange.com/questions/37920/how-do-i-set-up-networking-wifi-static-ip-address

## Ubuntu Server

https://www.techrepublic.com/article/how-to-configure-a-static-ip-address-in-ubuntu-server-18-04/


# References:

## Access Point

https://www.raspberrypi.org/documentation/configuration/wireless/access-point.md