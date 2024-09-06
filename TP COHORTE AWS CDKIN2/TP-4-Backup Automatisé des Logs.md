### Exercice 4: Backup Automatisé des Logs
1. **Objectif**: Automatiser la gestion des logs pour Node.js, Angular, et Apache.
2. **Étapes**:
   - Écrire un script Bash qui vérifie la taille des fichiers `.txt` et les sauvegarde dans un répertoire `backup/`.
   - Utiliser `tar` pour archiver les fichiers lorsqu'ils atteignent 2Ko.
3. **Code**:
   ```bash
   #!/bin/bash
   LOGFILE="logs.txt"
   BACKUP_DIR="backup/"
   MAXSIZE=2048  # 2KB

   if [ $(stat -c%s "$LOGFILE") -ge $MAXSIZE ]; then
       mv $LOGFILE "$BACKUP_DIR/logs_$(date '+%Y%m%d%H%M%S').txt"
       touch $LOGFILE
   fi
   ```