### Exercice 8: Sys Admin Mailing
1. **Objectif**: Envoyer les performances du serveur par email (CPU, mémoire, stockage).
2. **Étapes**:
   - Utiliser `mail` pour envoyer un rapport avec les commandes `free`, `df`, et `top`.
3. **Code**:
   ```bash
   #!/bin/bash
   echo "CPU usage: $(top -bn1 | grep 'Cpu(s)')" | mail -s "Server Performance" admin@example.com
   ```
