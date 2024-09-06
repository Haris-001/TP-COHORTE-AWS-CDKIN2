Exercice 11 : Load Balancer avec Nginx

### Objectif
Créer un **Load Balancer Nginx** qui distribue la charge sur plusieurs instances EC2 exécutant des serveurs Node.js dans un VPC, avec 3 sous-réseaux (1 public et 2 privés).

### Étapes

1. **Lancer trois instances EC2**
   - Créez 3 instances EC2 dans un **VPC** avec 3 **sous-réseaux** différents :
     - 1 instance dans un sous-réseau **public** pour Nginx.
     - 2 instances dans des sous-réseaux **privés** pour les serveurs Node.js.

2. **Installer Nginx sur l'instance publique**
   - Connectez-vous à l'instance publique et installez Nginx :
     ```bash
     sudo apt update
     sudo apt install nginx
     ```

   - Modifiez le fichier de configuration Nginx pour configurer le **load balancing** en **round-robin** :
     ```bash
     sudo nano /etc/nginx/nginx.conf
     ```

     Ajoutez cette configuration :
     ```nginx
     http {
         upstream node_servers {
             server IP_EC2_PRIVEE_1:5000;
             server IP_EC2_PRIVEE_2:5000;
         }

         server {
             listen 80;

             location / {
                 proxy_pass http://node_servers;
             }
         }
     }
     ```

   - Redémarrez Nginx :
     ```bash
     sudo systemctl restart nginx
     ```

3. **Configurer Node.js sur les instances privées**
   - Connectez-vous à chaque instance privée et installez Node.js :
     ```bash
     sudo apt update
     sudo apt install nodejs npm
     ```

   - Créez un serveur Node.js simple sur chaque instance :
     ```javascript
     const express = require('express');
     const app = express();
     const PORT = 5000;

     app.get('/', (req, res) => {
         res.send('Hello World from Instance');
     });

     app.listen(PORT, () => {
         console.log(`Server running on port ${PORT}`);
     });
     ```

   - Démarrez le serveur Node.js :
     ```bash
     node app.js
     ```

4. **Tester**
   - Accédez à l'IP publique de l'instance Nginx dans votre navigateur. Vous verrez un basculement entre les réponses des deux serveurs Node.js privés.

---