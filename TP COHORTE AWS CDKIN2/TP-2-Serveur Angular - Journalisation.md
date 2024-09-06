### Exercice 2: Serveur Angular - Journalisation
1. **Objectif**: Créer un projet Angular avec deux routes (`/home` et `/help`), et journaliser les visites.
2. **Étapes**:
   - Créer un projet Angular avec `ng new`.
   - Configurer Angular Router pour les deux routes.
   - Ajouter un service HTTP pour envoyer les journaux à un serveur Node.js (ou stocker en local).
3. **Code**: 
   - **Angular Routing** (app-routing.module.ts):
   ```typescript
   const routes: Routes = [
       { path: 'home', component: HomeComponent },
       { path: 'help', component: HelpComponent }
   ];
   ```
   - **Node.js backend**: Réutiliser le code du **Exercice 1** pour recevoir les requêtes HTTP et journaliser.