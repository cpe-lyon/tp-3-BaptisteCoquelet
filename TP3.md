
# Exercice 1. Gestion des utilisateurs et des groupes

1. Utilisez la commande groupadd pour créer deux groupes dev et infra
```bash
sudo groupadd dev
sudo groupadd infra
```
2. Créez ensuite 4 utilisateurs alice, bob, charlie, dave avec la commande useradd, en demandant la création de leur dossier personnel et avec bash pour shell
```bash
sudo useradd --shell /bin/bash -m alice
sudo useradd --shell /bin/bash -m bob
sudo useradd --shell /bin/bash -m charlie
sudo useradd --shell /bin/bash -m dave
```

3. Ajoutez les utilisateurs dans les groupes créés :
```bash
sudo usermod -a -G dev alice
sudo usermod -a -G dev bob
sudo usermod -a -G dev dave

sudo usermod -a -G infra bob
sudo usermod -a -G infra charlie
sudo usermod -a -G infra dave
```
4. Donnez deux moyens d’afficher les membres de infra
```batch
getent group infra
grep -i -n [nom du groupe recherché] /etc/group
```

5. Faites de dev le groupe propriétaire des répertoires /home/alice et /home/bob et de infra le groupe propriétaire de /home/charlie et /home/dave
```batch
chgrp dev /home/alice
chgrp dev /home/bob
  
chgrp infra /home/charlie
chgrp infra /home/dave
```
6. Remplacez le groupe primaire des utilisateurs
```batch
usermod alice -g dev
usermod bob-g dev

usermod charlie -g infra
usermod dave-g infra
```

7. Créez deux répertoires /home/dev et /home/infra pour le contenu commun aux membres de chaque groupe, et mettez en place les permissions leur permettant d’écrire dans ces dossiers.
```batch
mkdir /home/dev
mkdir /home/infra

chmod 070 /home/dev
chmod 070 /home/infra
```

8. Comment faire pour que, dans ces dossiers, seul le propriétaire d’un fichier ait le droit de renommer ou supprimer ce fichier ?
```batch
chmod +t [dossier]
```
9. Pouvez-vous ouvrir une session en tant que alice ? Pourquoi ?

on ne peut pas ouvrir de session en temps que "alice" car cette utilisateur n'est pas activité (ce compte n'a pas de mdp).

```batch
su alice
```

10. Activez le compte de l’utilisateur alice et vérifiez que vous pouvez désormais vous connecter avec son compte.
```batch
sudo passwd alice
sudo passwd --unlock alice
su alice
```
11. Comment obtenir l’uid et le gid de alice ?
```batch
id alice
```

12. Quelle commande permet de retrouver l’utilisateur dont l’uid est 1003 ?

```batch
id -nu 1003
charlie
```

  

13. Quel est l’id du groupe dev ?

```batch
id alice
```
on peut voire que l'id du groupe "dev" est 1002
  

14. Quel groupe a pour gid 1002 ?

```batch
getent group 1002
```
15. Retirez l’utilisateur charlie du groupe infra. Que se passe-t-il ? 

```batch
sudo gpasswd -d charlie infra
```
16. Modifiez le compte de dave

date d'expiration :
```batch
sudo chage -E 2023-06-01 dave
```
il faut changer de mot de passe avant 90 jours
```batch
sudo chage -M 90 dave
```
il faut attendre 5 jours pour modifier un mot de passe
```batch
sudo chage dave -m 5
```
envoyé une alerte 14 jour avant
```batch
sudo chage  -W 14 dave
```
le compte sera bloqué 30 jours après expiration du mot de passe
```batch
sudo chage dave-I 30
```

17. Quel est l’interpréteur de commandes (Shell) de l’utilisateur root ?

echo $SHELL

il s'agit de bash : '/bin/bash'.

  

18. Si vous regardez la liste des comptes présents sur la machine, vous verrez qu’il en existe un nommé nobody. A quoi correspond-il ?

  

L'utilisateur nobody sur un système Unix est utilisé par les serveurs NFS lorsqu'ils ne peuvent pas faire confiance aux UID et GID fournis par le client, ou lorsque l'option root-squash est utilisée.

  
  

19. Par défaut, combien de temps la commande sudo conserve-t-elle votre mot de passe en mémoire ? Quelle commande permet de forcer sudo à oublier votre mot de passe ?

Le mot de passe est mémorisé par défaut pour une durée de 15 minutes.
```
Sudo -K
```
  
  
  
  
  
  

# Exercice 2. Gestion des permissions

1. Dans votre $HOME, créez un dossier test, et dans ce dossier un fichier fichier contenant quelques lignes de texte. Quels sont les droits sur test et fichier ?

```
mkdir test
cd test
nano fichier
```

```
ls -l
```

**-rw-rw-r--**

  
2. Retirez tous les droits sur ce fichier (même pour vous), puis essayez de le modifier et de l’afficher en tant que root. Conclusion ?

```
sudo chmod 000 fichier
```

```
sudo nano fichier
sudo cat fichier
```

il y a la possibilité en root de passer outre les permission attribué.

  
3. Redonnez vous les droits en écriture et exécution sur fichier puis exécutez la commande echo "echo Hello" > fichier. On a vu lors des TP précédents que cette commande remplace le contenu d’un fichier s’il existe déjà. Que peut-on dire au sujet des droits ?
```
sudo chmod 777 fichier
```
```
echo "echo Hello" > fichier
```
  

4. Essayez d’exécuter le fichier. Est-ce que cela fonctionne ? Et avec sudo ?
```
./fichier
sudo ./fichier
```
Hello
"hello" s'affiche avec sudo.
  
5. Placez-vous dans le répertoire test, et retirez-vous le droit en lecture pour ce répertoire. Listez le contenu du répertoire, puis exécutez ou affichez le contenu du fichier fichier. Qu’en déduisez-vous ? Rétablissez le droit en lecture sur test.
```
sudo chmod 677 ~/test
ls
./fichier
```
Permission denied

```
sudo chmod 777 ~/test
```
  

6. Créez dans test un fichier nouveau ainsi qu’un répertoire sstest. Retirez au fichier nouveau et au répertoire test le droit en écriture. Tentez de modifier le fichier nouveau. Rétablissez ensuite le droit en écriture au répertoire test. Tentez de modifier le fichier nouveau, puis de le supprimer. Que pouvezvous déduire de toutes ces manipulations ?
```
touch nouveau
mkdir sstest
sudo chmod 555 nouveau
sudo chmod 555 ~/test
nano nouveau
```
permission denied
```
chmod 777 nouveau
nano nouveau
rm nouveaufonctionne
```
  

7. Positionnez vous dans votre répertoire personnel, puis retirez le droit en exécution du répertoire test. Tentez de créer, supprimer, ou modifier un fichier dans le répertoire test, de vous y déplacer, d’en lister le contenu, etc…Qu’en déduisez vous quant au sens du droit en exécution pour les répertoires ?
```
cd ~
sudo chmod 444 test
cd test
nano test/fichier
 ```
permission denied

 ```
ls test
```
cette commande fonctionne fonctionne

  

8. Rétablissez le droit en exécution du répertoire test. Positionnez vous dans ce répertoire et retirez lui à nouveau le droit d’exécution. Essayez de créer, supprimer et modifier un fichier dans le répertoire test, de vous déplacer dans ssrep, de lister son contenu. Qu’en concluez-vous quant à l’influence des droits que l’on possède sur le répertoire courant ? Peut-on retourner dans le répertoire parent avec ”cd ..” ? Pouvez-vous donner une explication ?
```
sudo chmod 777 test
cd test
sudo chmod 444 ~/test
```
  

9. Rétablissez le droit en exécution du répertoire test. Attribuez au fichier fichier les droits suffisants pour qu’une autre personne de votre groupe puisse y accéder en lecture, mais pas en écriture.
```
sudo chmod 777 ~/test
sudo chmod 710 test/fichier
```


10. Définissez un umask très restrictif qui interdit à quiconque à part vous l’accès en lecture ou en écriture, ainsi que la traversée de vos répertoires. Testez sur un nouveau fichier et un nouveau répertoire.

```
mkdir ~/test/test2
touch ~/test/fichier2
sudo umask 077 ~/test/test2
sudo umask 077 ~/test/fichier2
```

11. Définissez un umask très permissif qui autorise tout le monde à lire vos fichiers et traverser vos répertoires, mais n’autorise que vous à écrire. Testez sur un nouveau fichier et un nouveau répertoire.
```
mkdir ~/test/test3
touch ~/test/fichier3
sudo umask 022 ~/test/test3
sudo umask 022 ~/test/fichier3
```
12. Définissez un umask équilibré qui vous autorise un accès complet et autorise un accès en lecture aux membres de votre groupe. Testez sur un nouveau fichier et un nouveau répertoire.
```
mkdir ~/test/test4
touch ~/test/fichier4
sudo umask 067 ~/test/test4
sudo umask 067 ~/test/fichier4
```
13. Transcrivez les commandes suivantes de la notation classique à la notation octale ou vice-versa (vous pourrez vous aider de la commande stat pour valider vos réponses) :
```
chmod u=rx,g=wx,o=r fic
chmod 534 fic
```
```
chmod uo+w,g-rx fic
chmod 650 fic
```
```
chmod 653 fic
chmod rw-r-x-wx fic
```
```
chmod u+x,g=w,o-r fic
chmod 524 fic
```
