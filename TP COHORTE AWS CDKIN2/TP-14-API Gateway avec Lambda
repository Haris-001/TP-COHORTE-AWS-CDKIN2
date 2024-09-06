## Exercice 14 : API Gateway avec Lambda

### Objectif
Créer un site web avec deux pages (**login** et **register**), l'héberger dans S3, et utiliser **API Gateway** avec **AWS Lambda** pour gérer les appels API.

### Étapes

1. **Créer un site web avec deux pages (login, register)**
   - Créez un simple site HTML avec deux pages : `login.html` et `register.html`.
   - Hébergez le site dans un bucket S3 avec l'option **Static Website Hosting**.

2. **Lancer une instance EC2 pour MySQL**
   - Installez MySQL et configurez la base de données pour stocker les informations de login/register.

3. **Configurer Lambda pour gérer les requêtes**
   - Créez deux fonctions Lambda pour `login` et `register` avec la méthode POST.
   - Connectez Lambda à MySQL via **Secret Manager** pour récupérer les identifiants de connexion.

4. **Configurer API Gateway**
   - Créez une API dans **API Gateway** pour rediriger les requêtes vers les fonctions Lambda.

5. **Résoudre le problème CORS**
   - Activez CORS sur les API Gateway pour permettre les requêtes inter-origines.
