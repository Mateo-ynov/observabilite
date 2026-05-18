# Observabilité des systèmes
Contient l'ensemble des activités réalisées sur le module 'Observabilité des systèmes' sur la partie Loki

# Travaux Pratiques Grafana Loki & Grafana Alloy Docker
# Module 1 : Les fondamentaux

## Exercice 1 : Déploiement mono-nœud et vérification du flux de logs

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

14) Dans l'onglet Qurey de prometheus lancer la requête <em>node_cpu_seconds_total</em>. On obtient ainsi une séries de métriques qui prouvent bien que node_exporter est correctement scrapé par Prometheus :<br>
<img width="1143" height="494" alt="image" src="https://github.com/user-attachments/assets/c3599811-c36d-468a-b451-2e11d69cca4c" /><br>

## Exercice 4

15) Création d'un fichier targets.json contenant les deux endpoints (prometheus et node) avec <em>code targets.json</em><br>
<img width="515" height="452" alt="image" src="https://github.com/user-attachments/assets/8547727e-e39f-4e5b-9d36-b64eef017f64" /><br>

16) Modifier le fichier prometheus.yml pour monter targets.json sur <em>/etc/prometheus/sd/targets.json</em>. Pour cela le plus simple est de supprimer l'ancien conteneur prometheus avec <em>docker rm -f prometheus</em><br>

17) Dans le fichier prometheus.yml, les static_configs sont remplacer par file_sd_configs.<br>
<img width="471" height="337" alt="image" src="https://github.com/user-attachments/assets/3003be75-d145-4c9d-a246-c252b99a9feb" /><br>
Ainsi avec cela, Prometheus va directement aller lire les cibles depuis les fichiers JSON.<br>
Relancer le conteneur prometheus pour prendre en compe les nouvelles modifications.<br>
<img width="766" height="195" alt="image" src="https://github.com/user-attachments/assets/945c6a40-5814-4125-8194-6e2947b31d93" /><br>
Dans Status > Target on ne voit maintenant plus qu'un seul élément mais contenant les deux cibles.<br>
<img width="1158" height="268" alt="image" src="https://github.com/user-attachments/assets/e35489ca-640d-4f97-a8e9-504fd9c77670" /><br>

18) Afin de vérifier la prise en compte de prometheus sans rechargement. Modifier le fichier tragets.json en ne gardant plus que la partie node_exporter.<br>
Après avoir attendu pendant 5 secondes, on observe dans les targets que la cible correspondant a prometheus a disparue automatiquement. On ne voit plus que la cible "node". Cette modification a été effectué sans la nécessité de lancer un relaod.<br>
<img width="1155" height="237" alt="image" src="https://github.com/user-attachments/assets/ac5de846-335e-4042-a0c4-02959108a024" /><br>

## Exercice 5

19) Créer un répertoire rules avec <em>mkdir rules</em>. On a donc maintenant la structure suivante :
<img width="592" height="184" alt="image" src="https://github.com/user-attachments/assets/c35add6f-555c-42b3-9de6-258d9b04602e" /><br>
Créer un fichier api_rules.yml dans le répertoire rules. Celui-ci n'a qu'un seul groupe et qu'une seule qui dis que toutes les 30s, prometheus calcule le taux moyen sur 5 minutes de http_requests_total et l'enregistre dans une nouvelle métrique appelée job:http_requests:rate5m.<br>
<img width="694" height="224" alt="image" src="https://github.com/user-attachments/assets/9909ab30-8bd1-4bf0-a89f-6d6307b12b56" /><br>

20) Modifier le fichier prometheus.yml pour monter le répertoire sur <em>/etc/prometheus/rules</em>. Pour cela le plus simple est de supprimer l'ancien conteneur prometheus avec <em>docker rm -f prometheus</em><br>

21) Dans le fichier prometheus.yml ajouter une partie rule_files Le fichier est maintenant le suivant :
<img width="481" height="405" alt="image" src="https://github.com/user-attachments/assets/6f5b250d-62c1-45a6-afc4-8646b13afaec" /><br>
Comme il a été supprimé précédemment, on relance le conteneur prometheus avec le montage du dossier rules.
<img width="766" height="220" alt="image" src="https://github.com/user-attachments/assets/cf2b5b62-d485-4b6b-8c77-97e4cd373372" /><br>

22) Dans prometheus, dans Status > Rule, on observe l'apparition de la règle précédemment définie dans api_rules.yml.
<img width="1163" height="221" alt="image" src="https://github.com/user-attachments/assets/2287af81-2427-4d05-a6e8-7b84a4d40f07" /><br>

23) Afin de vérifier que celle-ci renvoie bien des données, dans l'onglet Query, interroger la nouvelle métrique <em>job:http_requests:rate5m</em>.<br>
<img width="1162" height="243" alt="image" src="https://github.com/user-attachments/assets/8db0f7f4-4d22-4cce-bd79-2cc5a70d71b8" /><br>
Pour générer du trafic avant d'effectuer le test, la commande lancée est : <em>for ($i=1; $i -le 20; $i++) {<br>
  curl.exe http://localhost:8000/api/users<br>
  curl.exe http://localhost:8000/api/orders<br>
}</em><br>

## Exercice 6

24) Dans le dossier /Prometheus, créer un fichier alertmanager.yml avec <em>code alertmanager.yml</em> dans lequel on mets le minimum par défaut. Le reste sera spécifié par la suite au lancement du conteneur<br>
<img width="704" height="203" alt="image" src="https://github.com/user-attachments/assets/46923f9b-20a1-40b6-a856-151eb5b20909" /><br>
On lance ensuite le conteneur alertmanager sur le port 9093 <img width="802" height="173" alt="image" src="https://github.com/user-attachments/assets/164bb4d3-b6c2-4540-8c9f-161aae9981a4" /><br>
On peut tester le lancement en testant : http://localhost:9093 sur un navigateur. On arrive donc à accéder à Alertmanager.<br>
<img width="1120" height="441" alt="image" src="https://github.com/user-attachments/assets/7da17839-c2d8-4782-9f75-d4d5bb85d6a5" /><br>

25) Dans le répertoire rules, modifier le fichier api_alerts.yml pour permettre la création d'une alerte HighErrorRate.<br>
<img width="948" height="470" alt="image" src="https://github.com/user-attachments/assets/4b8738c4-61d7-4028-88f5-6f540ef1dce9" /><br>
Cette règle permet de déclencher une alerte si les erreurs HTTP 5xx dépassent 5% du trafic de démo-api pendant 2 minutes.<br>

26)27) Dans le fichier prometheus.yml, vérifier la présence de rule_files et surtout ajouter une partie alerting qui permet d'envoyer les alertes vers Alertmanager.<br>
<img width="686" height="552" alt="image" src="https://github.com/user-attachments/assets/df6ee43a-969e-4bd8-9cf1-3a638f48a4be" /><br>

28) Pour prendre en compte les modifications effectuées, recharger prometheus avec <em>curl.exe -X POST http://localhost:9090/-/reload</em><br>
Sur Prometheus, dans Status > Rule on remarque l'apparition de la nouvelle règle nommée HighErrorRate.<br>
<img width="1160" height="220" alt="image" src="https://github.com/user-attachments/assets/49545e95-ca36-46bc-9947-73c147e57911" /><br>
Afin de déclencher l'alerte, il faut générer du trafic pendant plusieurs minutes à l'aide de la commande : <em>for ($i=1; $i -le 300; $i++) {<br>
  curl.exe -s http://localhost:8000/api/orders | Out-Null<br>
}<br></em>


29) Avec la génération de trafic, dans prometheus, l'alerte passe d'abord en pending : <img width="1151" height="414" alt="image" src="https://github.com/user-attachments/assets/489b1609-8592-41ff-b4c5-6f776c2e6e08" /><br>
Puis après 2 minutes en firing : <img width="1156" height="422" alt="image" src="https://github.com/user-attachments/assets/5e3b48eb-47c7-42e7-bdf8-a915f79d2670" /><br>
C'est à partir de ce moment là que l'alerte apparaît dans Alertmanager si la condition reste vraie après 2 minutes : <img width="1475" height="725" alt="image" src="https://github.com/user-attachments/assets/e2813110-246c-45df-9b57-956dfbbd347a" /><br>

## Exercice 7 

30) Dans cet exercice, tout va se passer depuis l'interface web de Prometheus dans l'onglet Query : <img width="1165" height="318" alt="image" src="https://github.com/user-attachments/assets/fd5c1d93-0bc2-4b56-ad91-98391b533da7" /><br>

31) La requête lancée est : <em>demo_http_requests_total</em>
<img width="1159" height="300" alt="image" src="https://github.com/user-attachments/assets/1242479e-4029-44a5-b644-aeecedfcc06a" /><br>
Le résultat est un vecteur instantané car la requête retourne la valeur acctuelle de chaque série temporelle demo_http_requests_total au moment de l'évaluation.<br>
