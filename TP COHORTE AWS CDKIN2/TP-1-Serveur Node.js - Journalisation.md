### Exercice 1: Serveur Node.js - Journalisation
1. **Objectif**: Créer un serveur Node.js avec deux routes (`/home` et `/help`) qui journalise chaque visite dans un fichier `.txt`.
2. **Étapes**:
   - Initialiser un projet Node.js (`npm init`).
   - Installer Express.js (`npm install express`).
   - Créer un serveur avec deux routes.
   - Utiliser le module `fs` de Node.js pour écrire dans un fichier `.txt` à chaque requête.
3. **Code**: 
   ```javascript
   const express = require('express');
   const fs = require('fs');
   const app = express();

   app.get('/home', (req, res) => {
       fs.appendFileSync('logs.txt', `Home visited: ${new Date()}\n`);
       res.send('Welcome to Home Page!');
   });

   app.get('/help', (req, res) => {
       fs.appendFileSync('logs.txt', `Help visited: ${new Date()}\n`);
       res.send('Welcome to Help Page!');
   });

   app.listen(3000, () => {
       console.log('Server is running on port 3000');
   });
   ```
   - **Commentaires**: Le fichier `logs.txt` enregistre chaque visite avec un horodatage.