# Observabilité des systèmes
Contient l'ensemble des activités réalisées sur le module 'Observabilité des systèmes'

## Exercice 1

1) Récupération de la dernière version de l'image docker de prometheus <em>docker pull prom/prometheus:latest</em>

2) Lancer le conteneur prometheus <em>docker run -d --name prometheus -p 9090:9090 prom/prometheus:latest</em><br>
<img width="992" height="325" alt="image" src="https://github.com/user-attachments/assets/e59cfb6b-1fed-41ff-8e88-03316ae291b9" /><br>

3) Validation avec l'ouverture dans un navigateur : http://localhost:9090<br>
<img width="1173" height="631" alt="image" src="https://github.com/user-attachments/assets/95daa9ad-1609-4682-b5b8-b7b17ed233ef" /><br>

4) Dans l'interface web, dans Status > Targets, validation de l'apparition de la cible prometheus et à l'état UP<br>
<img width="980" height="360" alt="image" src="https://github.com/user-attachments/assets/c4755aa4-d4ff-4db1-81c5-e4a52839a735" /><br>

5) Après exécution de la commande docker logs prometheus, on voit "Starting TSDB ...", puis "TSDB started" ce qui indique que c'est correctement démarré. On trouve également dans les logs <em>"filename=/etc/prometheus/prometheus.yml"</em>, donc le répertoire de stockage semble être <em>/etc/prometheus</em>.

## Exercice 2

6) Arrêt du conteneur prometheus précédemment créé <em>docker rm -f prometheus</em><br>

7) Création d'un fichier prometheus.yml <em>code prometheus.yml</em><br>
<img width="416" height="263" alt="image" src="https://github.com/user-attachments/assets/4147c58d-86bf-45af-968e-969cc5c6e7df" /><br>
Celui reprend les 3 éléments demandés :<br>
<ul>
  <li>définir un intervalle de scrapa global de 10 secondes avec <em>"scrape_interval: 10s"</em><br></li>
  <li>ajouter un label externe "environment=lab" avec <em>"external_labels: environment: lab"</em></li>
  <li>demander à prometheus de se surveiller lui-même avec <em>"targets: ["localhost:9090"]"</em></li>
</ul><br>

8) Lancement du nouveau conteneur à partir du prometheus.yml créé<br>
<img width="596" height="177" alt="image" src="https://github.com/user-attachments/assets/6c5f3ac8-d435-41c0-b6cd-1b174295886f" /><br>

9) Le fichier a été modifié, le scrape n'est plus de 10s mais de 5s. Un déclenchement de rechargement est mis en place <em>"curl.exe -X POST http://localhost:9090/-/reload"</em><br>

10) La modification a bien été prise en compte dans Status > Configuration, on observe que scrape_interval est passé à 5s<br>
<img width="239" height="163" alt="image" src="https://github.com/user-attachments/assets/72cacb15-9614-4295-964a-d79961187f72" />

## Exercice 3

11) Lancement de node_exporter <em>docker run -d --name node-exporter -p 9100:9100 prom/node-exporter:latest</em><br>
<img width="792" height="178" alt="image" src="https://github.com/user-attachments/assets/1dc4cfb1-fa60-4fb4-87a9-c2496d999a8f" /><br>

12) Ajout d'un nouveau job dans le prometheus.yml de la même manière que pour prometheus mais pour node<br>
<img width="476" height="84" alt="image" src="https://github.com/user-attachments/assets/1224dbf8-72cd-4108-9ff9-efaec458fe46" /><br>

13) Relancement du déclenchement de rechargement avec <em>curl.exe -X POST http://localhost:9090/-/reload</em><br>
Apparition de la cible Node en status UP dans Status > Target :<br>
<img width="1158" height="132" alt="image" src="https://github.com/user-attachments/assets/9c0d5ef2-826b-4812-9f6b-81d09899d2a3" /><br>

14) Dans l'onglet Qurey de prometheus lancer la requête <em>node_cpu_seconds_total</em>. On obtient ainsi une séries de métriques :<br>
<img width="1143" height="494" alt="image" src="https://github.com/user-attachments/assets/c3599811-c36d-468a-b451-2e11d69cca4c" /><br>

## Exercice 4









