## Exercice 16 : Application Load Balancer (ALB) avec Node.js

### Objectif
Configurer un **ALB** qui écoute sur le port 5000 et redirige le trafic vers une instance Node.js exécutant Express.

### Étapes

1. **Configurer une instance EC2 avec Node.js**
   - Suivez les étapes de l'Exercice 11 pour configurer un serveur Node.js sur port 5000.

2. **Créer un ALB**
   - Configurez l'ALB pour écouter sur le port 80 et rediriger vers le port 5000 sur votre instance EC2.

---