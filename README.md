# Observabilité des systèmes
Contient l'ensemble des activités réalisées sur le module 'Observabilité des systèmes' sur la partie Alloy

# Grafana Alloy & OpenTelemetry
# 1 : Fondamentaux

## Exercice 1  ·  Mettre Alloy en route

•	Objectif : déployer Alloy dans un namespace Kubernetes via Helm, avec un pipeline OTLP relié à un exporteur debug. Faire du port-forward sur l'UI pour inspecter le graphe.<br>

1) Création d'un namespace nommé 'observability' à l'aide de kubectl.<br>
<img width="695" height="197" alt="image" src="https://github.com/user-attachments/assets/fafb0199-721c-4808-8ad4-5837a151bcb2" /><br>

2) Création d'un fichier values.yaml.<br>
<img width="530" height="822" alt="image" src="https://github.com/user-attachments/assets/b00574a0-1f4f-413c-bea6-aed18ee6fdd6" /><br>


3) Installation de Alloy via Helm.<br>
<img width="823" height="217" alt="image" src="https://github.com/user-attachments/assets/7f08617c-cb1f-4044-9807-11e4c5b8ee33" /><br>

4) Vérification du lancement de Alloy.<br>
<img width="645" height="60" alt="image" src="https://github.com/user-attachments/assets/ca90d3f5-997a-494b-83da-93c7fa90444c" /><br>

5) Lancer un port-forward avec : "kubectl -n observability port-forward svc/alloy 12345:12345" et accéder à l'interface web de Grafana Alloy.<br>
<img width="942" height="228" alt="image" src="https://github.com/user-attachments/assets/026c9b2f-8115-4c72-a183-431d34c60a8f" /><br>

6) Réaliser un test depuis un deuxième terminal pour vérifier qu'Alloy est bien opérationnel.<br>
<img width="768" height="62" alt="image" src="https://github.com/user-attachments/assets/7925461e-ba23-4996-b296-3f3a65bfef48" /><br>

## Exercice 2  ·  Envoyer des données OTLP avec telemetrygen

•	Objectif : utiliser telemetrygen comme Pod Kubernetes éphémère pour pousser des données OTLP vers Alloy. Confirmer en suivant les logs Alloy.<br>

1) Avant de commencer il est nécessaire de s'assurer qu'Alloy tourne toujours.<br>

2) Création d'un pod temporaire pour générer des logs, des traces et des métriques à l'aide de 'telemetrygen'. C'est-à-dire, telemetrygen-traces, telemetrygen-logs et telemetrygen-metrics. Exemple pour les traces :<br>
<img width="678" height="186" alt="image" src="https://github.com/user-attachments/assets/f26fcc8d-ec09-4d16-9b83-794660dd3603" /><br>

3) Depuis un autre terminal, vérifier la bonne récupération des données.<br>
<img width="941" height="636" alt="image" src="https://github.com/user-attachments/assets/43092bbd-c3d5-4db9-ac13-990c8552144d" /><br>
 
## Exercice 3  ·  Instrumenter une vraie application avec le SDK OTel

•	Objectif : déployer une application Flask auto-instrumentée OpenTelemetry comme Deployment Kubernetes. L'app émet de l'OTLP vers Alloy au fil du trafic généré.<br>

1) Pour cet exercice, il est préférable de travailler dans un nouveau dossier car des fichiers app.py, requirements.txt, Dockerfile.<br>
<img width="542" height="492" alt="image" src="https://github.com/user-attachments/assets/54e6894a-00b0-40b3-99f6-2a9936b10750" />
<img width="402" height="156" alt="image" src="https://github.com/user-attachments/assets/14ce175a-2573-470c-8646-c696d5325d0b" />
<img width="715" height="311" alt="image" src="https://github.com/user-attachments/assets/aaf9bd8d-0a0f-4b72-87f4-a1525b29c61f" /><br>

2) Après cela, lancer le chargement de l'image dans le cluster : 'kind load docker-image flask-otel-demo:1.0 --name devsecops'.<br>

3) Créer le manifeste kubernetes dans lequel OTEL_SERVICE_NAME=demo-flask est le nom du service qui apparaît dans les logs Alloy, OTEL_EXPORTER_OTLP_ENDPOINT=http://alloy.observability.svc:4318 est l'adresse interne Kubernetes d’Alloy en OTLP HTTP et imagePullPolicy: Never pour dire d'utiliser l'image chargée dans le kind.<br>
<img width="587" height="846" alt="image" src="https://github.com/user-attachments/assets/82b22abd-ba6c-47bb-94af-af30a20bf42b" /><br>

4) Déployer l'application avec : 'kubectl apply -f k8s.yaml'.<br>

5) Faire le port-forward 'kubectl port-forward svc/demo 5000:5000' et vérifier la bonne connection.<br>
<img width="497" height="126" alt="image" src="https://github.com/user-attachments/assets/a2e4bf4b-48b0-471d-a02a-3491c22c6109" /><br>

6) Faire générer du traffic pour ensuite visualiser les logs Alloy.<br>
<img width="641" height="60" alt="image" src="https://github.com/user-attachments/assets/f351d995-aa95-4ccc-a244-5a8c0e38569d" /><br>

7) Depuis un deuxième terminal, vérifier les logs Alloy dans lequel on retrouve 'service.name: Str(demo-flask)', pour montrer que l'on attend bien ce qui est attendu.<br>
<img width="940" height="257" alt="image" src="https://github.com/user-attachments/assets/28e2758e-f6a3-495b-98da-a47d97399d6a" /><br>

## Exercice 4  ·  Maîtriser la syntaxe Alloy : pipeline, UI, hot reload

•	Objectif : étendre le pipeline Alloy avec une chaîne de processors entre le receiver OTLP et l'exporteur debug, observer le graphe en direct dans l'UI, et recharger la configuration sans redémarrer Alloy.
Pile en place depuis l'exercice 1 ; app de l'exercice 3 en train d'émettre des données.<br>

1) Le fichier values.yaml utilisé précédemment est modifié pour insérer deux processors entre le receiver OTLP et l’exporter debug.<br>
<img width="608" height="905" alt="image" src="https://github.com/user-attachments/assets/be8f0311-c20c-4b79-9848-96285722dd3e" /><br>

2) Appliquer la nouvelle configuration à l'aide d'Helm et vérifier que ça tourne toujours.<br>
<img width="833" height="302" alt="image" src="https://github.com/user-attachments/assets/7c662303-feaa-4a8a-9f9f-690967e00307" /><br>

3) Pour tester le bon fonctionnement du hot reload, lancer le port-forward vers Alloy et depuis un autre terminal effectuer un curl.<br>
<img width="823" height="41" alt="image" src="https://github.com/user-attachments/assets/c94ac199-f4fe-4f45-abe2-bf04a762a571" /><br>

4) Depuis un navigateur, vérifier que l'UI affiche les composants du pipeline Alloy. Le receiver OTLP reçoit les signaux, le processor batch les regroupe, le processor attributes ajoute l’attribut `deployment.environment=lab`, puis l’exporter debug affiche les signaux dans les logs Alloy.<br>
<img width="938" height="286" alt="image" src="https://github.com/user-attachments/assets/19f13325-332e-4a9d-a614-c06bbb6df041" /><br>

5) De la même manière que dans l'exercice précédent, vérifier dans un autre terminal que les logs contiennent 'deployment.environment: Str(lab)' avec 'kubectl -n observability logs deploy/alloy --tail=300 | grep "deployment.environment"'.<br>
<img width="941" height="177" alt="image" src="https://github.com/user-attachments/assets/b90e14e5-62f8-4325-8613-c82d2f3868a2" /><br>




