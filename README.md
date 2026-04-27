# Observabilité des systèmes
Contient l'ensemble des activités réalisées sur le module 'Observabilité des systèmes'

## Exercice 1

1) Récupération de la dernière version de l'image docker de prometheus : docker pull prom/prometheus:latest

2) Lancer le conteneur prometheus : docker run -d --name prometheus -p 9090:9090 prom/prometheus:latest<br>
<img width="992" height="325" alt="image" src="https://github.com/user-attachments/assets/e59cfb6b-1fed-41ff-8e88-03316ae291b9" /><br>

3) Validation avec l'ouverture dans un navigateur : http://localhost:9090<br>
<img width="1173" height="631" alt="image" src="https://github.com/user-attachments/assets/95daa9ad-1609-4682-b5b8-b7b17ed233ef" /><br>

4) Dans l'interface web, dans Status > Targets, validation de l'apparition de la cible prometheus et à l'état UP<br>
<img width="980" height="360" alt="image" src="https://github.com/user-attachments/assets/c4755aa4-d4ff-4db1-81c5-e4a52839a735" /><br>

5) Après exécution de la commande docker logs prometheus, on voit "Starting TSDB ...", puis "TSDB started" ce qui indique que c'est correctement démarré. On trouve également dans les logs "filename=/etc/prometheus/prometheus.yml", donc le répertoire de stockage semble être /etc/prometheus.

## Exercice 2

6) Arrêt du conteneur prometheus précédemment créé : docker rm -f prometheus<br>

7) Création d'un fichier prometheus.yml : code prometheus.yml<br>
<img width="416" height="263" alt="image" src="https://github.com/user-attachments/assets/4147c58d-86bf-45af-968e-969cc5c6e7df" /><br>
Celui reprend les 3 éléments demandés :<br>
<ul>
  <li>définir un intervalle de scrapa global de 10 secondes avec "scrape_interval: 10s"<br></li>
  <li>ajouter un label externe "environment=lab" avec "external_labels: environment: lab"</li>
  <li>demander à prometheus de se surveiller lui-même : "targets: ["localhost:9090"]"</li>
</ul> 

8) 
