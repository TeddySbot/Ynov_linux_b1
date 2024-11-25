# TP Avancé : "Mission Ultime : Sauvegarde et Sécurisation"

## Étape 1 : Analyse et nettoyage du serveur 

### Lister les tâches cron pour détecter des backdoors :

```powershell
crontab -l -u attacker

crontab -l -u hidden_data
```

### Identifier et supprimer les fichiers cachés :

```powershell
ls -a /tmp
rm maliscious.sh

ls -a /var/tmp
rm .nop

ls -a /home 
```

### Analyser les connexions réseau actives :

```powershell
netstat -a
```

## Étape 2 : Configuration avancée de LVM

### Créer un snapshot de sécurité pour /mnt/secure_data :

```powershell
lvcreate -L 500M -s -n tp4snapshot /dev/vg_secure/secure_data
```

### Tester la restauration du snapshot :

```powershell
rm /mnt/secure_data/sensitive1.txt

mount /mnt/secure_data/

lvconvert --mergesnapshot /dev/vg_secure/tp4snapshot

mount /mnt/secure_data

--------------------------------

umount /mnt/secure_data

lvchange --refresh vg_secure

mount /mnt/secure_data/
```

### Optimiser l’espace disque :

```powershell
lvextend --size +500M /dev/vg_secure/secure_data

lvs 
```

## Étape 3 : Automatisation avec un script de sauvegarde

### Créer un script secure_backup.sh :

```powershell
sudo nano secure_backup.sh
```

```sh
#!/bin/bash

