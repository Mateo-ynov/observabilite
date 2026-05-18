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


## Exercice 3



## Exercice 4



## Exercice 5


## Exercice 6


## Exercice 7 


