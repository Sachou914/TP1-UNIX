# TP 1 : Installation Serveurs

## Partie 1 - Installation Machine virtuelle

L’objectif de ce TP est de procéder à l’installation minimale d’un serveurs Linux.

Nous avons utilisé pour cela
le système de machines virtuelles VirtualBox avec la distribution Linux Debian stable. La machine virtuelle que nous avons généré possède processeur de type Amd64.


### Récupération Installeur
Dans notre cas de machines virtuelles est configurée pour booter sur cette miniso.
[Cliquer ici](https://ftp.lip6.fr/pub/linux/distributions/debian/dists/Debian12.7/main/installer-amd64/current/images/netboot/) pour récupérer le miniso.

### Installation 
Chaque étapes de l’installation est nécessaires allant de la création des utilisateurs à la partition des disques. Pour cette partie j'ai rencontré quelques difficulté mais j'ai trouvé et compris mes erreurs.


## Partie 2 - SSH
### 1. Installer le serveur SSH
Pour configurer SSH sur une machine Linux et permettre les connexions root à distance avec un mot de passe il faut faire les étapes suivantes :


- Cherchez les paquets SSH disponibles (pour vérifier si openssh-server est bien dans les dépôts) :

```
 apt search ssh
```

- Installez le serveur SSH :
```
 apt install openssh-server
```
### 2. Modifier la configuration de SSH

Une fois openssh-server installé, j'ai modifié le fichier de configuration SSH pour permettre les connexions root avec mot de passe.

J'ai édité le fichier de configuration SSH avec l'éditeur de texte nano :
``` bash
 nano /etc/ssh/sshd_config
``` 

Il faut ensuite recherchez les 2 ligne suivante pour les modifier :
 
``` 
#PermitRootLogin prohibit-password

#PasswordAuthentication yes
```

- La première ligne permet les connexions root avec mot de passe.
- La seconde ligne permet l'authentification par mot de passe.

Les lignes pour autoriser les connexions root avec mot de passe ainsi que l'authentification par mot de passe est commentée (précédée de #), il suffit des les décommenter en supprimant le # et mettre que sa valeur est bien yes :
```
PermitRootLogin yes

PaswordAuthentication yes
```
### 3. Redémarrer le service SSH

Après avoir effectué ces modifications, il faut redémarrer le service SSH pour que les changements soient appliqués avec la commande :

```
systemctl restart ssh
```

## Partie 3 - Connection et utilisation

### 1. Connection

Pour se connecter à la machine virtuelle depuis notre machine hôte il faut taper la commande suivante :

```
ssh root@<adresse_IP_de_votr_machine>
```
#### Si vous rencontrez des erreurs :

Pour s'assurer que le service est bien actif vous pouvez le vérifier en tapant la commande suivante dans la machine virtuelle :

```
systemctl status ssh
```
### 2. Utilisation des commandes

#### 2.1 Nombre de paquets
La commande suivante permet de connaitre le nombre de paquet de l'installation :
```
dpkg -l | wc -l
```
Ma machine possède 352 paquets, une installation légère requière peu d'espace.

#### 2.2 Space Usage

La commande ``` df -h ``` en Linux est utilisée pour afficher l'espace disque utilisé et disponible sur les systèmes de fichiers montés, avec des tailles formatées de manière lisible pour l'humain.

#### Exemple de sortie :

``` 
Sys. de fichiers Taille Utilisé Dispo Uti% Monté sur
udev               965M       0  965M   0% /dev
tmpfs              197M    564K  197M   1% /run
/dev/sda1          9,1G    1,3G  7,4G  15% /
tmpfs              984M       0  984M   0% /dev/shm
tmpfs              5,0M       0  5,0M   0% /run/lock
/dev/sda3          921M     70M  788M   9% /var/log
/dev/sda2          3,6G     40K  3,4G   1% /tmp
tmpfs              197M       0  197M   0% /run/user/0

``` 
- Sys. de fichiers : Le système de fichiers monté.
- Taille : Taille totale du système de fichiers.
- Utilisé : Espace disque déjà utilisé.
- Dispo : Espace encore disponible.
- Uti% : Pourcentage de l'espace utilisé.
- Monté sur : Point de montage où le système de fichiers est accessible dans l'arborescence.

Cela permet de voir rapidement l'état de l'espace disque disponible et utilisé sur différents systèmes de fichiers de manière claire.

### 3. Quelques commandes expliquées et résultats

#### 1. La commande ```echo $LANG``` 

```echo``` affiche la valeur actuelle de la variable $LANG

``` $LANG ``` est une variable utilisé dans les systèmes Unix et Linux pour spécifier la langue, le pays et les préférences de codage des caractères.

##### Résultat obtenu :

``` 
fr_FR.UTF-8
 ```

#### 2. La commande ```hostname```

La commande hostname permet d'afficher le nom de la machine dans les systèmes.

##### Résultat obtenu :

``` 
serveur1
 ```
#### Suite
Il est également possible de changer le nom de la machine avec la même commande suivie du nouveau nom.
##### Résultat obtenu :

``` 
hostname <nouveauNom>
```
#### 3. La commande ```hostname -d```

Pour afficher le domaine de la machine avec la commande on peut consulter le manuel en utilisant la commande ```man hostname```.

L'option la plus courante pour affciher ```-d``` ou ```--domain```

```
hostname -d
```

##### Résultat obtenu :

``` 
ufr-info-p6.jussieu.fr
 ```



#### 4. La commande ```cat /etc/apt/sources.list | grep -v -E '^#|^$'```
##### 1. ```cat /etc/apt/sources.list``` :

- La commande cat permet d'afficher le contenu du fichier ```/etc/apt/sources.list```.
- Ce fichier contient la liste des dépôts (serveurs) où APT (Advanced Package Tool) va chercher les logiciels et les mises à jour.


##### 2. ```grep -v -E '^#|^$'``` :

- ```grep``` est utilisé pour filtrer les lignes du fichier.
- ```-v``` : Cette option inverse la sélection, c'est-à-dire que seules les lignes qui ne correspondent pas au motif seront affichées.
- ```-E``` : Active les expressions régulières étendues (permet d'utiliser des métacaractères comme ```|```).
- ```'^#'``` : Correspond à toutes les lignes qui commencent par un #, qui sont des commentaires dans le fichier.
- ```'^$'``` : Correspond à toutes les lignes vides.
- Ainsi, la commande supprime les lignes vides et les commentaires, ne laissant que les lignes actives contenant les URL des dépôts.

##### Résultat obtenu :

``` 
deb http://ftp.lip6.fr/pub/linux/distributions/debian/ bookworm main
deb-src http://ftp.lip6.fr/pub/linux/distributions/debian/ bookworm main
deb http://security.debian.org/debian-security bookworm-security main
deb-src http://security.debian.org/debian-security bookworm-security main
deb http://ftp.lip6.fr/pub/linux/distributions/debian/ bookworm-updates main
deb-src http://ftp.lip6.fr/pub/linux/distributions/debian/ bookworm-updates main
 ```
#### 5. La commande ```cat /etc/shadow | grep -vE ':\*:|:!\*:'```

La commande est utilisée pour filtrer et afficher uniquement les lignes pertinentes du fichier ```/etc/shadow```, en excluant celles qui contiennent des informations spécifiques sur les comptes verrouillés ou désactivés. Le fichier /etc/shadow contient les mots de passe et les informations sur les comptes des utilisateurs dans les systèmes Linux.

```cat /etc/shadow :```

- La commande ```cat``` permet d'afficher le contenu du fichier ```/etc/shadow```.
- Ce fichier contient des informations sensibles sur les utilisateurs, notamment les mots de passe chiffrés, les dates de dernière modification du mot de passe, etc.
- Seuls les utilisateurs avec des privilèges root peuvent visualiser ce fichier.

```grep -vE ':\*:|:!\*:'``` :

- ```grep``` est utilisé pour rechercher des motifs spécifiques dans le fichier.
- ```-v``` : Inverse la sélection, c'est-à-dire que seules les lignes qui ne correspondent pas au motif seront affichées.
- ```-E``` : Active les expressions régulières étendues, ce qui permet d'utiliser des métacaractères comme | pour signifier "ou".
- ```:\*:``` et ```:!\*: ``` :
  - ``` :\*:``` : Correspond aux comptes d'utilisateurs désactivés (les mots de passe sont remplacés par un astérisque ```*```).
  - ```:!\*:``` : Correspond aux comptes verrouillés.
- Le but de cette commande est d'exclure (via ```-v```) les lignes où les comptes sont désactivés ou verrouillés, car ces utilisateurs ne peuvent pas se connecter.

##### Résultat obtenu :

```
root:$y$j9T$Z5VvXnsVlpR.gwYgNp9MjTHQfaGQOURkkyYHfdgp2i9C1U2C:19998:0:99999:7:::
messagebus:!:19998::::::
avahi-autoipd:!:19998::::::
sshd:!:19998::::::
``` 

#### 6. La commande ```cat /etc/passwd | grep -vE 'nologin|sync'```

```cat /etc/passwd``` :

La commande cat affiche le contenu du fichier ```/etc/passwd```. Ce fichier contient des informations sur les comptes utilisateurs du système.

```grep -vE 'nologin|sync'```  :

- ```grep```  : Utilisé pour rechercher du texte dans un fichier.

- ```-v```  : Inverse la recherche. Cela signifie que seules les lignes qui ne correspondent pas au motif spécifié seront affichées.
- ```-E```  : Active les expressions régulières étendues, permettant d'utiliser le symbole ```|```  pour signifier "ou".
- ```'nologin|sync'```  :  Les deux motifs recherchés sont ```nologin```  ou ```sync``` , qui apparaissent dans le champ du shell de l'utilisateur.
  - ```nologin```  : Indique que l'utilisateur ne peut pas se connecter au système (shell désactivé).
  - ```sync```  : Utilisé pour les comptes systèmes spécialisés dans les opérations de synchronisation, ce compte ne permet pas non plus de connexion interactive.

##### Résultat obtenu :

```
root:x:0:0:root:/root:/bin/bash
``` 
#### Explications :
- L'utilisateurs root a un shell interactif (/bin/bash ), ce qui signifie qu'il peut se connecter au système et utiliser un terminal.
- Tous les utilisateurs avec un shell nologin ou sync ont été exclus du résultat.
- Cela permet d'identifier rapidement les comptes utilisateurs actifs, c'est-à-dire ceux qui peuvent se connecter au système de manière interactive.
- Les comptes avec nologin ou sync sont généralement des comptes système ou des comptes de service non interactifs.

#### 6. Les commandes  ```fdisk -l```  et ```fdisk -x```

```fdisk -l``` :

- La commande ```fdisk -l``` est utilisée pour lister toutes les partitions et les disques présents sur le système.

Explication de la commande ```fdisk -l``` :

- ```fdisk``` est un utilitaire de partitionnement qui permet de manipuler les partitions de disques durs sur Linux.

- ```-l``` (list) : Cette option affiche une liste de toutes les partitions disponibles sur tous les disques détectés par le système.


##### Résultat obtenu :

```
Disque /dev/sda : 20 GiB, 21474836480 octets, 41943040 secteurs
Modèle de disque : VBOX HARDDISK
Unités : secteur de 1 × 512 = 512 octets
Taille de secteur (logique / physique) : 512 octets / 512 octets
taille d'E/S (minimale / optimale) : 512 octets / 512 octets
Type d'étiquette de disque : gpt
Identifiant de disque : 475518F4-2CA0-41CF-B356-72FD6CA7A763

Périphérique    Début      Fin Secteurs Taille Type
/dev/sda1        2048 19531775 19529728   9,3G Système de fichiers Linux
/dev/sda2    19531776 27344895  7813120   3,7G Système de fichiers Linux
/dev/sda3    27344896 29298687  1953792   954M Système de fichiers Linux
/dev/sda4    29298688 41940991 12642304     6G Partition d'échange Linux
``` 


```fdisk -x``` :

- La commande ```fdisk -x``` sert à afficher des informations détaillées sur la table de partitions d'un disque utilisant le format GPT (GUID Partition Table). Elle fournit des détails supplémentaires par rapport à la commande classique ```fdisk -l```.

Explication de la commande ```fdisk -x``` :
- ```fdisk``` est un utilitaire de partitionnement qui permet de manipuler les partitions de disques durs sur Linux.

- ```-x```  : Cette option est identique à --list, mais avec plus de détails comme UUID ou le nom.

##### Résultat obtenu :

```
Disque /dev/sda : 20 GiB, 21474836480 octets, 41943040 secteurs
Modèle de disque : VBOX HARDDISK
Unités : secteur de 1 × 512 = 512 octets
Taille de secteur (logique / physique) : 512 octets / 512 octets
taille d'E/S (minimale / optimale) : 512 octets / 512 octets
Type d'étiquette de disque : gpt
Identifiant de disque: 475518F4-2CA0-41CF-B356-72FD6CA7A763
Premier LBA utilisable: 34
Dernier LBA utilisable: 41943006
LBA alternatif: 41943039
LBA de départ des entrées de partition: 2
Entrées de partitions allouées: 128
LBA de fin des entrées de partition: 33

Périphérique    Début      Fin Secteurs Type-UUID                            UUID                                 Nom Attr.
/dev/sda1        2048 19531775 19529728 0FC63DAF-8483-4772-8E79-3D69D8477DE4 F4C8BD6B-4486-442A-ACDD-CC0E19FCF2DD la racine

/dev/sda2    19531776 27344895  7813120 0FC63DAF-8483-4772-8E79-3D69D8477DE4 D7F07BBA-71E6-4BAC-96C6-5CC7A52174A8
/dev/sda3    27344896 29298687  1953792 0FC63DAF-8483-4772-8E79-3D69D8477DE4 A78BE967-2B5E-4BC8-B05D-6D32457FBFB8
/dev/sda4    29298688 41940991 12642304 0657FD6D-A4AB-43C4-84E5-0933C84B4F4F 940ABE54-B5B2-44B5-8715-FB26C7583FDF swap
```
#### 6. La commande ```df -h```
La commande ```df -h``` en Linux est utilisée pour afficher l'espace disque utilisé et disponible sur les systèmes de fichiers montés. 

Explication de la commande ```df -h``` :

- ```df``` : "disk free" — affiche les informations sur l'espace disque utilisé et disponible.
- ```-h``` : Formatage des valeurs en unités lisibles par l'humain (Ko, Mo, Go, To).
##### Résultat obtenu :

``` 
Sys. de fichiers Taille Utilisé Dispo Uti% Monté sur
udev               965M       0  965M   0% /dev
tmpfs              197M    564K  197M   1% /run
/dev/sda1          9,1G    1,3G  7,4G  15% /
tmpfs              984M       0  984M   0% /dev/shm
tmpfs              5,0M       0  5,0M   0% /run/lock
/dev/sda3          921M     78M  780M  10% /var/log
/dev/sda2          3,6G     40K  3,4G   1% /tmp
tmpfs              197M       0  197M   0% /run/user/0
``` 


#### Détails du retour de la commande :

- Filesystem (Système de fichiers) :

  - Indique l'appareil ou la partition utilisée pour stocker les données.
  - Par exemple, /dev/sda1 correspond à la première partition du disque dur principal.

- Size (Taille) :

  - La taille totale du système de fichiers ou de la partition, formatée en unités lisibles (ici en Go et Mo).
  - Par exemple, /dev/sda1 a une taille totale de 10 Go.

- Used (Utilisé) :

  - Montre l'espace disque déjà utilisé par les fichiers et dossiers sur cette partition.
 
- Avail (Disponible) :

  - Affiche l'espace restant disponible pour les nouvelles données.
Sur /dev/sda1, il reste  

- Use% (Pourcentage d'utilisation) :

  - Indique le pourcentage de l'espace disque déjà utilisé sur chaque système de fichiers.

- Mounted on (Monté sur) :

  - Indique le point de montage où le système de fichiers est accessible dans le système.

### 4. Aller plus loin

### Installation automatique

La commande ```preseed``` est utilisée pour automatiser les installations de systèmes d'exploitation basées sur Debian. C'est un mécanisme permettant de fournir à l'installateur des réponses à l'avance, afin de spécifier à l'avance certaines options de configuration comme :

- La sélection du fuseau horaire.
- Le partitionnement du disque.
- La configuration du réseau.
- Les utilisateurs à créer.
- Les paquets supplémentaires à installer.

### Rescue mode


Si vous avez oublié le mot de passe root et que vous souhaitez le réinitialiser sur un système Linux, vous pouvez utiliser le mode de récupération (rescue mode) pour réinitialiser le mot de passe root. 

Voici les étapes détaillées pour le faire :

#### Utiliser le mode de récupération (Rescue Mode) via GRUB

#### 1. Redémarrez l'ordinateur :

Si vous êtes déjà connecté à la session, redémarrez le système avec la commande suivante :

```
reboot
```

#### 2. Accédez au menu GRUB :

Pendant le démarrage, maintenez la touche Shift enfoncée (ou la touche Esc dans certains systèmes) pour afficher le menu GRUB.

#### 3. Éditez les options du noyau :

Dans le menu GRUB, sélectionnez l'option de démarrage de votre système (habituellement la première entrée) puis appuyez sur la touche ```e``` pour éditer les options de démarrage du noyau.

#### 4. Modifiez la ligne de démarrage :

 Recherchez la ligne qui commence par linux et qui contient des informations sur le chemin du noyau et les options de démarrage.
À la fin de cette ligne, remplacez les paramètres de démarrage en ajoutant ```init=/bin/bash```.

#### 5. Démarrez avec ces options :

Après avoir ajouté ```init=/bin/bash```, appuyez sur Ctrl + X ou F10 pour démarrer avec ces paramètres modifiés.

#### 6. Réinitialisez le mot de passe root :

 Une fois que vous arrivez dans un shell (bash ou sh), le système sera monté en mode lecture seule. Il est nécessaire de le remonter en lecture-écriture pour effectuer les changements de mot de passe.
Remontez la racine du système de fichiers en lecture-écriture avec cette commande :
```
mount -o remount,rw /
```
 Ensuite, réinitialisez le mot de passe root avec la commande :
```
passwd root
```
Il vous sera demandé de saisir un nouveau mot de passe root.

#### 7. Redémarrez le système :

Une fois le mot de passe root réinitialisé, vous pouvez redémarrer le système avec :
```
exec /sbin/init
```
Le système redémarrera normalement, et vous pourrez vous connecter avec le nouveau mot de passe root.

### Redimensionnement partition

#### 1. Vérifiez l'espace disponible :

 Avant de redimensionner, assurez-vous d'avoir suffisamment d'espace sur le disque. Vous pouvez utiliser la commande suivante :

```
df -h
```
#### 2. Redimensionner le système de fichiers

Si vous avez de l'espace non alloué sur votre disque après la partition racine, vous pouvez simplement étendre le système de fichiers en utilisant resize2fs.

```
resize2fs /dev/sda1
```

Pour réduire la taille de la partition racine, il faut d’abord réduire le système de fichiers, puis redimensionner la partition elle-même.

```
resize2fs /dev/sda1 15G
```
Cela réduit le système de fichiers à 15 Go. 

Réduisez ensuite la partition elle-même. Pour cela, vous utiliserez ```parted``` :

```
parted /dev/sda
```

```
resizepart 1 15G
```

Une fois dans l’interface de ```parted```, vous pouvez réduire la partition :

```
resizepart 1 15G
```
#### 3. Vérifiez le système de fichiers
Après avoir redimensionné le système de fichiers ou la partition, vous devriez vérifier l'intégrité du système de fichiers :
```
fsck /dev/sda1
```

Pour terminer, il suffit de redémarrer le système pour s'assurer que tout fonctionne correctement.

```
reboot
```



# 
### Sacha CLEMENT 
### TP01 Unix
# 


