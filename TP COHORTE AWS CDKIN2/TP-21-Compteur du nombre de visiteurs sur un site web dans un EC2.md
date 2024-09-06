## Exercice 21 : Compteur du nombre de visiteurs sur un site web dans un EC2

### Objectif
Créer un site web simple hébergé sur une instance EC2 qui affiche le nombre de visiteurs. Le nombre de visites sera enregistré dans un fichier texte.

### Étapes

1. **Lancer une instance EC2**
   - Utilisez la console AWS pour lancer une instance EC2 avec un groupe de sécurité autorisant le trafic HTTP (port 80).

2. **Installer Node.js et Nginx**
   ```bash
   sudo apt update
   sudo apt install nodejs npm nginx
   ```

3. **Créer une application Node.js pour suivre les visiteurs**
   - **app.js** :
     ```javascript
     const express = require('express');
     const fs = require('fs');
     const app = express();
     const PORT = 80;

     let visitCount = 0;

     // Charger le nombre de visites depuis le fichier texte
     fs.readFile('visits.txt', 'utf8', (err, data) => {
         if (!err && data) {
             visitCount = parseInt(data);
         }
     });

     app.get('/', (req, res) => {
         visitCount++;
         fs.writeFile('visits.txt', visitCount.toString(), (err) => {
             if (err) {
                 console.error('Erreur lors de l\'enregistrement des visites');
             }
         });
         res.send(`Nombre de visiteurs: ${visitCount}`);
     });

     app.listen(PORT, () => {
         console.log(`Serveur en cours d'exécution sur le port ${PORT}`);
     });
     ```

4. **Démarrer le serveur**
   ```bash
   node app.js
   ```

5. **Tester**
   - Accédez à l'IP publique de l'instance EC2 dans un navigateur et observez l'incrémentation du compteur à chaque visite.

---