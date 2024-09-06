### Exercice 9: Backup S3 avec EC2
1. **Objectif**: Créer des archives avec `tar` et les sauvegarder dans un bucket S3.
2. **Étapes**:
   - Lancer une instance EC2 et créer un bucket S3.
   - Utiliser IAM pour donner accès S3.
   - Écrire un script pour automatiser le backup avec `tar` et `aws s3 cp`.
3. **Code**:
   ```bash
   tar -cvf backup.tar /path/to/files
   aws s3 cp backup.tar s3://mybucket/
   ```