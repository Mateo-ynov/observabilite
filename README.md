# Observabilité des systèmes
Contient l'ensemble des activités réalisées sur le module 'Observabilité des systèmes' sur la partie Loki

# Travaux Pratiques Grafana Loki & Grafana Alloy Docker
# Module 1 : Les fondamentaux

## Exercice 1 : Déploiement mono-nœud et vérification du flux de logs

•	Objectif : Déployer une instance fonctionnelle de Grafana, Loki, et Grafana Alloy.<br>
•	Tâche : Créer un fichier docker-compose.yml incluant Loki, Grafana et Alloy. Configurer Grafana Alloy pour collecter les fichiers de logs locaux ou de conteneurs et les acheminer vers Loki. Connecter Grafana à Loki et valider le flux via l'onglet Explore.<br>

1) Création du fichier docker-compose.yml incluant Loki, Grafana et Alloy.
<img width="701" height="871" alt="image" src="https://github.com/user-attachments/assets/65b98709-196b-404a-aebd-649f1c5ff4d1" /><br>

2) Création d'un fichier 'config.alloy' pour la configuration de Grafana Alloy.
<img width="633" height="782" alt="image" src="https://github.com/user-attachments/assets/bde5268a-b8e0-4465-b628-c8ec3f291ac5" /><br>

3) Déploiement des conteneurs avec "docker compose up -d"
<img width="1307" height="112" alt="image" src="https://github.com/user-attachments/assets/ef4d7257-e460-48ed-bb0a-395867c6be66" /><br>
Trois conteneurs sont maintenant lancés : alloy, loki et grafana.
<img width="845" height="252" alt="image" src="https://github.com/user-attachments/assets/0a260395-c4ec-4722-8f86-3020f20a089a" />

4) Ajouter Loki en source de données dans Grafana
<img width="918" height="501" alt="image" src="https://github.com/user-attachments/assets/1ee75ebc-f7da-49e5-9c95-fec61d89d6e2" /><br>

5) Vérification des logs dans Explore
<img width="1918" height="851" alt="image" src="https://github.com/user-attachments/assets/cbceee4e-6712-41e5-a1e5-f87435f7c7ac" /><br>

6) Test avec des logs tests générées via une commande
<img width="1205" height="180" alt="image" src="https://github.com/user-attachments/assets/6b8d9d46-a931-4f3c-937d-ca1fb9c3d967" /><br>

7) Le test est validé, on retrouve dans l'Explore de Grafana les éléments du test lancé.
<img width="1917" height="770" alt="image" src="https://github.com/user-attachments/assets/5e034e05-7a39-4767-94f0-32c4a4a03fb0" /><br>

## Exercice 2 : Comprendre les labels et le mécanisme de Relabeling

•	Objectif : Assimiler la gestion des labels par Grafana Alloy et l'indexation par Loki.<br>
•	Tâche : Modifier la configuration Alloy pour supprimer un label natif (ex. container_id ou filename). Ajouter un label statique 'environment=development' et un label dynamique 'loglevel' extrait des métadonnées du flux de logs. Vérifier le résultat dans Grafana.<br>

1) Modification du fichier config.alloy pour la configuration de Grafana Alloy. Cette mofification a pour but de supprimer un label nabel natif, ajouter un label statique 'environment=development' et un label dynamique 'loglevel' extrait des métadonnées du flux de logs.
<img width="493" height="930" alt="image" src="https://github.com/user-attachments/assets/10b4352a-ba9a-4d49-af6d-5e4faf9b8f3d" /><br>

2) Pour prendre en compte la modification, il est nécessaire de relancer la stack. Pour cela d'abord la stopper avec 'docker compose down'. Puis relancer le déploiement avec 'docker compose up -d'.

3) Vérifier que tous les conteneurs sont bien relancés et accessibles
<img width="1370" height="483" alt="image" src="https://github.com/user-attachments/assets/07e0edcb-592b-4757-8e5c-f3141b4f34a0" /><br>

4) Si tout est bon, effectuer un test avec une commande qui génère des messages d'erreurs
<img width="1307" height="201" alt="image" src="https://github.com/user-attachments/assets/b1b4009c-7eac-49ea-bf79-d3e2db85d4be" /><br>

5) S'assurer que cela remonte bien dans Explore dans Grafana.<br>
Dans l'exemple, un filtre est fait sur tous les messages de type 'ERROR'.
<img width="1902" height="831" alt="image" src="https://github.com/user-attachments/assets/438f51f3-7608-4130-8d74-5ef5cc97b6a0" /><br>

## Exercice 3 : Rotation des logs et découverte dynamique

•	Objectif : Garantir une collecte continue et sans doublons lors des événements de rotation de fichiers.<br>
•	Tâche : Configurer Alloy pour suivre un répertoire (/var/log/apps/*.log). Simuler l'activité d'une application, puis déclencher manuellement une rotation de fichiers (renommage et création d'un nouveau fichier vide). Confirmer qu'Alloy conserve sa position de lecture sans perte.<br>

1) Pour débuter, créer un répertoire 'apps-logs' qui contiendra l'ensemble des logs.<br>
Pour la suite, mettre dans ce répertoire un fichier 'app-log' contenant quelques lignes, par exemple :
<img width="463" height="87" alt="image" src="https://github.com/user-attachments/assets/f7965311-0315-425f-99f4-5281777beca6" /><br>

2) Modifier le service alloy dans le docker-compose.yml pour ajouter le volume correspondant à 'apps-logs'.
<img width="496" height="96" alt="image" src="https://github.com/user-attachments/assets/c3be7d73-d1d4-4fbd-a82e-054bfb59fa1e" /><br>

3) Modifier le contenu du fichier 'config.alloy' par une version plus orientée fichiers
<img width="461" height="455" alt="image" src="https://github.com/user-attachments/assets/94d7cff7-be58-483c-8e6e-c83dd69dffdb" /><br>
Cette configuration permet de demander à Alloy de suivre tous les fichiers ".log" présents dans "/var/log/apps/".

4) Pour prendre en compte ces configurations, il est nécessaire de relancer la stack.

5) Vérifier ensuite que les données du fichier de log remontent bien dans Grafana.
<img width="1913" height="617" alt="image" src="https://github.com/user-attachments/assets/f83e64a5-321a-4284-aaab-dcf6fdaa861c" /><br>

6) Pour faire un test de rotation, lancer directement en shell les commandes suivantes : "Rename-Item .\apps-logs\app2.log app2.log.1
New-Item .\apps-logs\app2.log -ItemType File -Force
Add-Content .\apps-logs\app2.log "ligne 3 - apres rotation"<br>
S'assurer que cela apparaît ensuite dans Grafana sans écraser ce qu'il y a eu précédemment.
<img width="1738" height="590" alt="image" src="https://github.com/user-attachments/assets/4eb238d6-07d6-4f82-a215-8f5b2f76fe6c" /><br>

## Exercice 4 : Pipelines Alloy et Parsing à la source

•	Objectif : Transformer, filtrer et enrichir les structures de logs directement au niveau de la couche de collecte.<br>
•	Tâche : Produire des logs fictifs au format JSON. Configurer un composant 'loki.process' dans Grafana Alloy pour parser ce JSON, extraire le champ 'user_id' en tant que label temporaire, et supprimer toutes les lignes de niveau 'debug' avant l'envoi à Loki.<br>

1) Pour cette partie, on remodifie le contenu du fichier 'config.alloy'. Dans ce nouveau fichier, stage.json parse les logs JSON, user_id est extrait depuis le JSON, stage.labels transforme user_id en label Loki et stage.drop supprime les logs dont level="debug".
<img width="461" height="922" alt="image" src="https://github.com/user-attachments/assets/8de9c22e-430e-4834-aa67-f8883c9b9182" /><br>

2) Relancer le service Alloy pour prendre en compte la configuration : 'docker compose restart alloy'.

3) Dans Grafana, vérifier que les informations apparaissent, par exemple en filtrant sur un user.
<img width="1903" height="717" alt="image" src="https://github.com/user-attachments/assets/73db330e-676a-4f4d-88a8-c9f82be37002" /><br>

4) On peut également vérifier que les lignes de niveau 'debug' ont été supprimées.
<img width="1918" height="408" alt="image" src="https://github.com/user-attachments/assets/4fbf17f5-176e-4cab-a50d-671135d744a3" /><br>

## Exercice 5


## Exercice 6


## Exercice 7 


