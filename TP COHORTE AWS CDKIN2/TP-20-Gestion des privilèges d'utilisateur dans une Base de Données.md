## Exercice 20 : Gestion des privilèges d'utilisateur dans une Base de Données

### Objectif
En tant qu'administrateur de base de données, vous devez créer des tables et configurer les privilèges d'un utilisateur nommé **Victoria** avec certaines restrictions.

### Étapes

1. **Créer les tables nécessaires**
   - Utilisez MySQL pour créer les tables suivantes :
     - **RESET**
     - **EMPLOYES**
     - **DEPARTMENTS**
     - **TACHES**

   ```sql
   CREATE TABLE RESET (
       id INT AUTO_INCREMENT PRIMARY KEY,
       reset_code VARCHAR(255)
   );

   CREATE TABLE EMPLOYES (
       id INT AUTO_INCREMENT PRIMARY KEY,
       name VARCHAR(255),
       password VARCHAR(255)
   );

   CREATE TABLE DEPARTMENTS (
       id INT AUTO_INCREMENT PRIMARY KEY,
       department_name VARCHAR(255)
   );

   CREATE TABLE TACHES (
       id INT AUTO_INCREMENT PRIMARY KEY,
       task_name VARCHAR(255),
       task_description TEXT
   );
   ```

2. **Créer l'utilisateur Victoria**
   ```sql
   CREATE USER 'victoria'@'%' IDENTIFIED BY 'secure_password';
   ```

3. **Accorder des privilèges spécifiques**
   - **Ne pas permettre l'accès à la table RESET** :
     ```sql
     REVOKE ALL PRIVILEGES ON RESET FROM 'victoria'@'%';
     ```

   - **Autoriser la vue des employés sans la colonne `password`** :
     ```sql
     GRANT SELECT (id, name) ON EMPLOYES TO 'victoria'@'%';
     ```

   - **Autoriser la liste des départements, sans possibilité d'INSERT ni d'UPDATE** :
     ```sql
     GRANT SELECT ON DEPARTMENTS TO 'victoria'@'%';
     ```

   - **Autoriser l'insertion dans la table `TACHES`, sans possibilité de modifier ou de supprimer** :
     ```sql
     GRANT SELECT, INSERT ON TACHES TO 'victoria'@'%';
     REVOKE UPDATE, DELETE ON TACHES FROM 'victoria'@'%';
     ```

4. **Vérification des privilèges**
   - Connectez-vous en tant que `victoria` pour vérifier les restrictions appliquées.

---