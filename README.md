
 SSH : 
Commandes utilisées :

Recherche et installation du serveur SSH :

    apt search ssh
    apt install openssh-server


Activation et vérification du statut de SSH :

        systemctl enable --now ssh.service
        systemctl status sshd



Connexion SSH :
J’ai configuré une redirection de ports pour la VM. Voici la commande utilisée pour se connecter :

    ssh root@localhost -p 2222

Informations système

Nombre de paquets installés :

dpkg -l | wc -l

Le résultat de cette commande indique 334 paquets.

Adresse IP :
Pour obtenir les informations réseau, j’ai utilisé :

    ip addr show

2.5 Paramètres régionaux

La commande :

    echo $LANG

affiche le résultat suivant : fr_FR.UTF-8, indiquant que la langue par défaut est le français avec l’encodage UTF-8.

Nom de la machine :
La commande :

    hostname

renvoie le nom de la machine, ici serveur1.

Domaine :
J’ai utilisé hostname -d pour vérifier le domaine, et cela renvoie : ufr-info-p6.jussieu.fr.

Dépôts et comptes système

Vérification des dépôts :

    cat /etc/apt/sources.list | grep -v -E '^#|^$'

Cette commande liste les dépôts utilisés pour les mises à jour du système :

    deb http://deb.debian.org/debian/ bookworm main
    deb http://security.debian.org/debian-security bookworm-security main

Comptes utilisateurs et shadow :

•	La commande :

    cat /etc/shadow | grep -vE ':\*:|:!\*:'

renvoie les informations de comptes sécurisés du système.

•	Pour vérifier les comptes utilisateurs, j’ai utilisé :

    cat /etc/passwd | grep -vE 'nologin|sync'



Gestion des disques

Informations sur les disques :

•	Commande pour afficher les disques :

    fdisk -l

•	Elle montre les partitions actives sur le système, incluant leurs tailles et types.

•	La commande df -h :

    df -h

Affiche l’espace disque utilisé, où l’option -h permet d’avoir une lecture en format humain (Go, Mo).

3.2 Mode Rescue

Changer le mot de passe root :

1.	Accéder à GRUB en appuyant sur Shift lors du démarrage.
2.	Modifier l’entrée de démarrage en appuyant sur e et ajouter init=/bin/bash.
3.	Une fois dans le shell, remontez la partition racine :

        mount -o remount,rw /


4.	Changer le mot de passe root :

        passwd root


5.	Redémarrez avec :

        reboot



3.3 Redimensionnement de partition

Étapes pour redimensionner la partition :

1.	Vérification de l’espace disque :

        df -h

Vérifiez l’espace disponible avant toute modification.

2.	Mode Single-User :
Démarrez en mode single via GRUB pour que la partition racine ne soit pas utilisée.
	3.	Redimensionnement avec fdisk :
Utilisez fdisk pour supprimer puis recréer la partition avec les nouvelles dimensions, sans toucher aux secteurs de début pour éviter la perte de données.
	4.	Redimensionner le système de fichiers :

            resize2fs /dev/sda1


	5.	Vérification finale :
Confirmez les modifications avec df -h pour voir l’espace alloué.

