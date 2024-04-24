# backupsystem
Script de sauvegarde backupsystem
> Utilisation :
*sudo backupsystem [-0] [-C] [-H] [-R] [-v] [-a] [-A] [-c] [-V] [-X] [-h] [-D] [-d] [-m] [-f myfile.conf]*

> Description :
	Ce script doit être lancé avec les privilèges du superutilisateur root (voir les commandes sudo ou su).
	Il permet une sauvegarde incrémentielle COMPLÈTE de votre système,
	ainsi qu'une sauvegarde incrémentielle d'un dossier SPÉCIFIQUE (par exemple /home).
	Une sauvegarde ADDITIONNELLE des données suivantes sera également réalisée :
		- les fichiers crontab,
		- la liste des paquets installés (paquets de la distribution et éventuels paquets Flatpak),
		- la liste des fichiers du système.
	Le script peut effectuer la sauvegarde sur un démon rsyncd distant ou dans un dossier (point de montage local ou distant).

> Dossier de configuration : /etc/backupsystem
> Fichiers de configuration :
	- backupsystem.conf, contient les options du script tel que, par exemple, le dossier de sauvegarde SPÉCIFIQUE.
	- exclusion-full.txt, contient la liste des dossiers et fichiers exclus de la sauvegarde COMPLÈTE.
	- exclusion.txt, contient la liste des dossiers et fichiers exclus de la sauvegarde SPÉCIFIQUE.
Au premier lancement du script, il va créer des modèles des fichiers de configuration à ajuster à votre système.

> Options disponibles :
	
```
-h: affiche cette aide,
-v: affiche le numéro de version & quitte,
-c: vérifie & affiche la configuration, et contrôle que toutes les commandes nécessaires installées,
-0: détruit le log à la fin du processus (mode "nolog"),
-V: affiche le log à la fin de la sauvegarde,
-X: affiche la sortie de la commande rsync pour suivre la progression de la sauvegarde,
-m: envoie le log par email à la fin de la sauvegarde, nécessite l'installation et la configuration d'un logiciel smtp (comme msmtp ou ssmtp),
-D: détruit toutes les sauvegardes de la machine,
-a: lance une sauvegarde puis crée une archive tar.zst du dossier de sauvegarde,
-A: lance une sauvegarde puis crée une archive tar.zst du dossier de sauvegarde et la transfère dans le cloud,
-d: efface tous les logs de la machine,
-R: restaure les sauvegardes COMPLÈTE et SPÉCIFIQUE dans les dossiers définis (point de montage local ou distant),
-C: pour ne pas faire la sauvegarde COMPLÈTE et la sauvegarde ADDITIONNELLE,
-H: pour ne pas faire la sauvegarde SPÉCIFIQUE,
-f myfile.conf: pour utiliser un fichier de configuration alternatif (myfile.conf).‘
```
> Une tâche cron peut être ajoutée pour automatiser des sauvegardes :
	
```
        - Par exemple, ajouter la ligne suivante dans /etc/crontab pour une sauvegarde automatique toutes les 6 heures
	0 */6 * * * root /usr/local/bin/backupsystem

	- Ou bien, pour une sauvegarde automatique mensuelle le 1er à 20h10
	(en utilisant le fichier de conf ~/myfile.conf et en envoyant le log par email)
	10 20 1 * * root /usr/local/bin/backupsystem -m -f ~/myfile.conf

	- Ou bien, pour une sauvegarde SPÉCIFIQUE automatique quotidienne à minuit
	0 0 * * * root /usr/local/bin/backupsystem -C

	- Ou bien, pour créer une archive "tar.zst" de la sauvegarde tous les lundis à midi (pas de sauvegarde préalable)
	0 12 * * 1 root /usr/local/bin/backupsystem -C -H -a
```
