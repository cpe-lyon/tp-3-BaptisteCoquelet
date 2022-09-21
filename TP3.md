# Exercice 1. Gestion des utilisateurs et des groupes
1)
```batch
sudo groupadd dev
sudo groupadd infra
```
2)
```batch
sudo useradd --shell /bin/bash -m alice
sudo useradd --shell /bin/bash -m bob
sudo useradd --shell /bin/bash -m charlie
sudo useradd --shell /bin/bash -m dave
```

3)

```batch
sudo usermod -a -G dev alice
sudo usermod -a -G dev bob
sudo usermod -a -G dev dave 

sudo usermod -a -G infra bob
sudo usermod -a -G infra charlie
sudo usermod -a -G infra dave 
```
4)
```batch
getent group infra

grep -i -n [nom du groupe recherché] /etc/group
```
5)
```batch
chgrp dev /home/alice
chgrp dev /home/bob

chgrp infra /home/charlie
chgrp infra /home/dave
```
6)
```batch
usermod alice -g dev
usermod bob-g dev

usermod charlie -g infra
usermod dave-g infra
```
7)
```batch
mkdir /home/dev
mkdir /home/infra

chmod 070 /home/dev
chmod 070 /home/infra
```

8)
Pour que dans ces dossiers seul le propriétaire d’un fichier ait le droit de renommer ou supprimer ce fichier il faut activé le sticky bit sur le dossier :
```batch
chmod +t [dossier]
```

9)
on ne peut pas ouvrir de session en temps que "alice" car cette utilisateur n'est pas activité (ce compte n'a pas de mdp).
```batch
su alice
```

10)
```batch
sudo passwd alice
sudo passwd --unlock alice
su alice
```

11)
```batch
id alice
```
12)
```batch
id -nu 1003
charlie
```

13)
```batch
id alice
```
on peut voire que l'id du groupe "dev" est 1002

14)
```batch
getent group 1002
```
15)
```batch
sudo gpasswd -d charlie infra
```
16)
TODO
date d'expiration :
sudo chage -E 2023-06-01 dave


 il faut changer de mot de passe avant 90 jours
sudo chage -M 90 dave

il faut attendre 5 jours pour modifier un mot de passe
sudo chage dave -m 5


envoyé une alerte 14 jour avant
sudo chage -W 14 dave

le compte sera bloqué 30 jours après expiration du mot de passe
sudo chage dave-I 30

17)
echo $SHELL
/bin/bash

18)

L'utilisateur nobody sur un système Unix est utilisé par les serveurs NFS lorsqu'ils ne peuvent pas faire confiance aux UID et GID fournis par le client, ou lorsque l'option root-squash est utilisée. 


19)
Le mot de passe est mémorisé par défaut pour une durée de 15 minutes.
Sudo -K






# Exercice 2. Gestion des permissions
1)
