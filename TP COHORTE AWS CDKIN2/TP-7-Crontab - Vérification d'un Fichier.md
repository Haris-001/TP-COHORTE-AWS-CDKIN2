### Exercice 7: Crontab - Vérification d'un Fichier
1. **Objectif**: Vérifier l'existence de `server.conf` et le télécharger avec `git` s'il n'existe pas.
2. **Étapes**:
   - Utiliser `git clone` ou `wget` pour télécharger le fichier si nécessaire.
3. **Code**:
   ```bash
   if [ ! -f server.conf ]; then
       git clone https://github.com/user/repo.git
   fi
   ```