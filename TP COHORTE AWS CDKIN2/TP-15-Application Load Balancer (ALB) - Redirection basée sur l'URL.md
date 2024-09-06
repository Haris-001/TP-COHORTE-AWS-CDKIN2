## Exercice 15 : Application Load Balancer (ALB) - Redirection basée sur l'URL

### Objectif
Déployer un **Application Load Balancer** (ALB) qui redirige le trafic vers différents groupes cibles en fonction de l'URL (`/user` et `/admin`).

### Étapes

1. **Créer deux groupes cibles (Target Groups)**
   - L'un pour `/user` et l'autre pour `/admin`, chacun avec son propre **Auto Scaling Group**.

2. **Créer un ALB**
   - Dans AWS, configurez un ALB qui redirige vers les groupes cibles en fonction de l'URL.

---