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

### Créer un script secure_backup.sh & Ajoutez une fonction de rotation des sauvegardes :

```powershell
sudo nano secure_backup.sh
```

```sh
#!/bin/bash
DATE=$(date +%Y%m%d%H%M)
BACKUP="/backup/secure_data_$DATE.tar.gz"
SAUVE="/mnt/secure_data"
FILE_COUNT=£$(ls -1 | wc -1)

tar -czf $BACKUP $SAUVE

cd  /backup/


if [ $file -gt 7 ]; then
    ls -t | sed -e '1,7d' | xargs -d '\n' rm -f
else 
    echo error
fi
```

### Testez le script :

```powershell 
/secure_backup.sh
```

### Automatisez avec une tâche cron :

```powershell

[powershell]
crontab -e

[Vi]
0 0 3 * * * /secure_backup.sh

[command Vi]
:x 
```

## Étape 4 : Surveillance avancée avec auditd

### Configurer auditd pour surveiller /etc :

```powershell
nano /etc/audit/rules.d/audit.rules
``` 

```
-w /etc -p wa -k surveillance-etc
```

### Tester la surveillance :
```
ausearch -k surveillance-etc
```

### Analyser les événements :
```
echo "$(ausearch -k surveillance-etc -i)" > /var/log/audit_etc.log
```
## Étape 5 : Sécurisation avec Firewalld

### Configurer un pare-feu pour SSH et HTTP/HTTPS uniquement :

```
firewall-cmd --permanent --add-service=http
firewall-cmd --permanent --reload
```

### Bloquer des IP suspectes :

### Restreindre SSH à un sous-réseau spécifique :



