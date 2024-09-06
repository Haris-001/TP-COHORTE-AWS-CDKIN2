## Exercice 12 : Balance des charges avec réplication Master-Master

### Objectif
Configurer un load balancer Nginx pour équilibrer la charge sur deux bases de données **MySQL** répliquées en **Master-Master**.

### Étapes

1. **Lancer trois instances EC2**
   - 1 instance publique avec Nginx (load balancer).
   - 2 instances privées avec MySQL, configurées pour la réplication Master-Master.

2. **Configurer MySQL Master-Master**
   - Installez MySQL sur chaque instance privée :
     ```bash
     sudo apt update
     sudo apt install mysql-server
     ```

   - Configurez chaque instance comme **Master** :
     - Modifiez `/etc/mysql/mysql.conf.d/mysqld.cnf` pour définir l'ID du serveur et activer la réplication :
       ```bash
       server-id = 1  # Changez l'ID pour la deuxième instance
       log_bin = /var/log/mysql/mysql-bin.log
       ```
     - Redémarrez MySQL :
       ```bash
       sudo systemctl restart mysql
       ```

   - Créez un utilisateur pour la réplication et configurez la réplication Master-Master :
     ```sql
     CREATE USER 'replicator'@'%' IDENTIFIED BY 'password';
     GRANT REPLICATION SLAVE ON *.* TO 'replicator'@'%';
     ```

3. **Configurer Nginx**
   - Modifiez Nginx pour équilibrer la charge entre les deux serveurs MySQL.

4. **Tester la réplication**
   - Insérez des données sur une instance et vérifiez qu'elles sont répliquées sur l'autre.

---