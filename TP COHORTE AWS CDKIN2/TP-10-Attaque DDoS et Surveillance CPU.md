### Exercice 10: Attaque DDoS et Surveillance CPU

### Objectif
Créer deux instances EC2 dans AWS. Sur la première instance, vous allez simuler une attaque DDoS en envoyant des requêtes massives à la seconde instance. Sur la seconde instance, vous allez surveiller l'utilisation CPU chaque seconde et consigner ces données dans un fichier CSV.

À la fin de l'attaque, vous pourrez analyser les variations de la charge CPU et visualiser les résultats sous forme de graphique.

### Étapes détaillées

#### 1. Créer deux instances EC2
- Lancez deux instances EC2 dans la même région, avec un système d'exploitation Ubuntu ou Amazon Linux 2.
- Assurez-vous que les instances peuvent communiquer entre elles (via des Security Groups). Ouvrez le port 80 pour recevoir des requêtes HTTP sur l'instance cible (Instance 2).
- Prenez note des adresses IP publiques des deux instances.

#### 2. Instance 1: Simuler l'attaque DDoS

- Connectez-vous à la première instance EC2 (Instance 1).
- Installez `curl` si ce n'est pas déjà fait :
  ```bash
  sudo apt-get update
  sudo apt-get install curl
  ```

- Créez un script `ddos_attack.sh` qui envoie un grand nombre de requêtes à l'instance cible (Instance 2) en boucle :

  **Script: `ddos_attack.sh`**
  ```bash
  #!/bin/bash
  TARGET_IP="IP_DE_L_INSTANCE_2"  # Remplacez par l'adresse IP de la deuxième instance

  echo "Démarrage de l'attaque DDoS contre $TARGET_IP"
  while true; do
      curl -s http://$TARGET_IP &  # Envoyer une requête HTTP et l'exécuter en arrière-plan
  done
  ```

- Rendez le script exécutable :
  ```bash
  chmod +x ddos_attack.sh
  ```

- Exécutez le script :
  ```bash
  ./ddos_attack.sh
  ```

> Ce script va envoyer continuellement des requêtes HTTP à l'instance cible, simulant une attaque DDoS.

#### 3. Instance 2: Surveiller la charge CPU

- Connectez-vous à la seconde instance EC2 (Instance 2).
- Créez un script `monitor_cpu.sh` pour surveiller l'utilisation du CPU chaque seconde et enregistrer les résultats dans un fichier `.csv`.

  **Script: `monitor_cpu.sh`**
  ```bash
  #!/bin/bash
  OUTPUT_FILE="cpu_usage.csv"
  echo "Timestamp,CPU_Usage(%)" > $OUTPUT_FILE  # En-tête du fichier CSV

  while true; do
      CPU_USAGE=$(top -bn1 | grep "Cpu(s)" | awk '{print $2 + $4}')  # Calculer l'utilisation CPU
      TIMESTAMP=$(date '+%Y-%m-%d %H:%M:%S')  # Obtenir l'horodatage
      echo "$TIMESTAMP,$CPU_USAGE" >> $OUTPUT_FILE  # Écrire dans le fichier CSV
      sleep 1  # Attendre une seconde avant de mesurer à nouveau
  done
  ```

- Rendez le script exécutable :
  ```bash
  chmod +x monitor_cpu.sh
  ```

- Exécutez le script pour commencer la surveillance CPU :
  ```bash
  ./monitor_cpu.sh
  ```

> Ce script surveille l'utilisation du CPU toutes les secondes et stocke les résultats dans le fichier `cpu_usage.csv`.

#### 4. Observer les logs en temps réel

- Sur Instance 2, utilisez `tail -f` pour observer l'augmentation de l'utilisation CPU en temps réel :
  ```bash
  tail -f cpu_usage.csv
  ```

#### 5. Arrêter l'attaque DDoS

- Une fois que vous avez recueilli suffisamment de données (par exemple après quelques minutes d'attaque), arrêtez l'attaque sur Instance 1 en appuyant sur `CTRL+C`.

#### 6. Analyser les données

- Sur Instance 2, le fichier `cpu_usage.csv` contiendra une liste horodatée de l'utilisation du CPU. Téléchargez ce fichier sur votre machine locale pour analyse.
- Utilisez un outil comme **Excel** ou **Google Sheets** pour tracer les variations d'utilisation du CPU en fonction du temps et analyser l'impact de l'attaque DDoS.

#### Exemple de fichier `cpu_usage.csv`
```
Timestamp,CPU_Usage(%)
2024-09-06 14:01:01,5.5
2024-09-06 14:01:02,7.8
2024-09-06 14:01:03,25.0
2024-09-06 14:01:04,45.3
2024-09-06 14:01:05,65.2
```

#### 7. Visualiser les données

- Utilisez les données du fichier CSV pour tracer un graphique de l'utilisation du CPU au fil du temps. Cela vous permettra de voir clairement les pics d'utilisation pendant l'attaque DDoS.
- Vous pouvez utiliser des outils comme **Matplotlib** en Python pour visualiser les données si vous préférez coder la visualisation.

**Exemple de code Python avec Matplotlib :**
```python
import matplotlib.pyplot as plt
import pandas as pd

# Charger le fichier CSV
data = pd.read_csv('cpu_usage.csv')

# Tracer le graphique
plt.plot(data['Timestamp'], data['CPU_Usage(%)'])
plt.xticks(rotation=45)  # Rotation des étiquettes de l'axe X pour plus de lisibilité
plt.xlabel('Timestamp')
plt.ylabel('CPU Usage (%)')
plt.title('Variation de l\'utilisation CPU pendant une attaque DDoS')
plt.show()
```

---

### Conclusion

Dans cet exercice, vous avez simulé une attaque DDoS entre deux instances EC2 et surveillé les performances du CPU de l'instance cible. Grâce à la surveillance des ressources système et à l'analyse des données, vous avez pu observer l'impact de l'attaque sur la charge CPU et tirer des conclusions sur la robustesse de l'infrastructure face à de telles menaces.