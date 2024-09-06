### Exercice 13 : Balance des charges avec réplication Master-Slave

### Objectif
Configurer un **load balancer** Nginx pour équilibrer la charge entre deux serveurs MySQL configurés en **réplication Master-Slave**. Cela permet de distribuer les requêtes de lecture sur le Slave et d'effectuer les opérations d'écriture sur le Master.

### Étapes

#### 1. Lancer trois instances EC2

- **Instance 1** : Serveur Nginx (Load Balancer) dans un sous-réseau public.
- **Instance 2** : Serveur MySQL Master dans un sous-réseau privé.
- **Instance 3** : Serveur MySQL Slave dans un sous-réseau privé.

Assurez-vous que les instances peuvent communiquer entre elles via les **Security Groups** en autorisant les connexions sur le port 3306 pour MySQL.

#### 2. Installer MySQL sur les instances Master et Slave

Sur **Instance 2 (Master)** et **Instance 3 (Slave)**, installez MySQL :

```bash
sudo apt update
sudo apt install mysql-server
```

#### 3. Configurer MySQL Master

Sur l'instance **Master (Instance 2)** :

1. **Modifier le fichier de configuration MySQL** :

   - Ouvrez le fichier `/etc/mysql/mysql.conf.d/mysqld.cnf` et configurez les paramètres suivants :

   ```bash
   server-id = 1                # Identifiant unique du serveur Master
   log_bin = /var/log/mysql/mysql-bin.log  # Fichier de logs binaires pour la réplication
   binlog_do_db = your_database_name       # Nommez la base de données à répliquer
   ```

2. **Redémarrer MySQL** :

   ```bash
   sudo systemctl restart mysql
   ```

3. **Configurer les permissions pour la réplication** :

   - Connectez-vous à MySQL :

     ```bash
     sudo mysql -u root -p
     ```

   - Créez un utilisateur de réplication et accordez-lui les privilèges nécessaires :

     ```sql
     CREATE USER 'replicator'@'%' IDENTIFIED BY 'password';
     GRANT REPLICATION SLAVE ON *.* TO 'replicator'@'%';
     FLUSH PRIVILEGES;
     ```

4. **Verrouiller la base de données pour les instantanés** :

   - Cette étape garantit que vous récupérerez un état cohérent de la base de données avant de commencer la réplication :

     ```sql
     FLUSH TABLES WITH READ LOCK;
     ```

5. **Obtenir l'état actuel du Master** :

   - Pour que le Slave puisse se synchroniser, vous devez noter la position actuelle du fichier binaire :

     ```sql
     SHOW MASTER STATUS;
     ```

   - Prenez note du `File` et de la `Position`. Vous en aurez besoin pour configurer le Slave.

   - Déverrouillez les tables après avoir pris les informations :

     ```sql
     UNLOCK TABLES;
     ```

#### 4. Configurer MySQL Slave

Sur **Instance 3 (Slave)** :

1. **Modifier le fichier de configuration MySQL** :

   - Ouvrez le fichier `/etc/mysql/mysql.conf.d/mysqld.cnf` et configurez les paramètres suivants :

   ```bash
   server-id = 2                # Identifiant unique du serveur Slave (doit être différent du Master)
   relay_log = /var/log/mysql/mysql-relay-bin.log  # Fichier de logs de relay pour le Slave
   ```

2. **Redémarrer MySQL** :

   ```bash
   sudo systemctl restart mysql
   ```

3. **Configurer le Slave pour qu'il se synchronise avec le Master** :

   - Connectez-vous à MySQL sur le Slave :

     ```bash
     sudo mysql -u root -p
     ```

   - Configurez la réplication avec les informations obtenues du Master (`File` et `Position`) :

     ```sql
     CHANGE MASTER TO
     MASTER_HOST='IP_DE_L_INSTANCE_MASTER',
     MASTER_USER='replicator',
     MASTER_PASSWORD='password',
     MASTER_LOG_FILE='nom_du_fichier_binaire',  # Le nom du fichier obtenu avec SHOW MASTER STATUS
     MASTER_LOG_POS=position;  # La position obtenue
     ```

4. **Démarrer la réplication sur le Slave** :

   ```sql
   START SLAVE;
   ```

5. **Vérifier l'état de la réplication** :

   - Pour vérifier que la réplication fonctionne correctement :

     ```sql
     SHOW SLAVE STATUS\G;
     ```

   - Assurez-vous que `Slave_IO_Running` et `Slave_SQL_Running` sont tous les deux sur **Yes**.

#### 5. Configurer le Load Balancer avec Nginx

Sur l'**Instance 1 (Nginx)** :

1. **Installer Nginx** :

   ```bash
   sudo apt update
   sudo apt install nginx
   ```

2. **Configurer Nginx pour le load balancing** :

   - Ouvrez le fichier de configuration Nginx `/etc/nginx/nginx.conf` et ajoutez la configuration suivante :

   ```nginx
   http {
       upstream mysql_servers {
           server IP_DE_L_INSTANCE_MASTER:3306;  # Redirection des écritures vers le Master
           server IP_DE_L_INSTANCE_SLAVE:3306 backup;  # Le Slave est en backup pour les lectures
       }

       server {
           listen 80;

           location / {
               proxy_pass http://mysql_servers;
               proxy_set_header Host $host;
               proxy_set_header X-Real-IP $remote_addr;
               proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
           }
       }
   }
   ```

3. **Redémarrer Nginx** :

   ```bash
   sudo systemctl restart nginx
   ```

#### 6. Tester la configuration

1. **Vérification de la réplication MySQL** :
   - Connectez-vous à la base de données sur le **Master** (Instance 2) et insérez quelques enregistrements dans la base de données répliquée.
   - Sur le **Slave** (Instance 3), vérifiez que ces enregistrements sont bien répliqués.

2. **Tester le Load Balancer** :
   - Configurez une application (ou utilisez des requêtes directes) qui effectue des opérations de **lecture** (SELECT) et **écriture** (INSERT/UPDATE) sur le Load Balancer.
   - Vérifiez que les écritures sont bien redirigées vers le Master et que les lectures sont possibles via le Slave.

---

### Conclusion

Vous avez configuré un environnement MySQL avec une réplication **Master-Slave** et un **Load Balancer Nginx** pour répartir la charge entre ces serveurs. Le Master gère toutes les écritures, tandis que le Slave est utilisé pour les lectures, offrant ainsi une répartition de la charge et une haute disponibilité.