### Exercice 3: Serveur Apache - Journalisation
1. **Objectif**: Journaliser les visites sur un serveur Apache avec des pages PHP (`index.php` et `help.php`).
2. **Étapes**:
   - Configurer Apache pour servir des pages PHP.
   - Modifier les fichiers `index.php` et `help.php` pour écrire les journaux dans un fichier `.txt`.
3. **Code**:
   ```php
   // index.php
   file_put_contents('logs.txt', "Visited Home: " . date('Y-m-d H:i:s') . "\n", FILE_APPEND);
   echo "Welcome to Home Page!";
   ```
