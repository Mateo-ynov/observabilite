# Observabilité des systèmes
Contient l'ensemble des activités réalisées sur le module 'Observabilité des systèmes'

## Exercice 1

1) Récupération de la dernière version de l'image docker de prometheus : docker pull prom/prometheus:latest

2) Lancer le conteneur prometheus : docker run -d --name prometheus -p 9090:9090 prom/prometheus:latest
<img width="992" height="325" alt="image" src="https://github.com/user-attachments/assets/e59cfb6b-1fed-41ff-8e88-03316ae291b9" /><br>

3) Validation avec l'ouverture dans un navigateur : http://localhost:9090
<img width="1173" height="631" alt="image" src="https://github.com/user-attachments/assets/95daa9ad-1609-4682-b5b8-b7b17ed233ef" /><br>

4) Dans l'interface web, dans Status > Targets, validation de l'apparition de la cible prometheus et à l'état UP
<img width="980" height="360" alt="image" src="https://github.com/user-attachments/assets/c4755aa4-d4ff-4db1-81c5-e4a52839a735" /><br>

5) Après exécution de la commande docker logs prometheus, on voit "Starting TSDB ...", puis "TSDB started" ce qui indique que c'est correctement démarré. On trouve également dans les logs "filename=/etc/prometheus/prometheus.yml", donc le répertoire de stockage semble être /etc/prometheus.

## Exercice 2

