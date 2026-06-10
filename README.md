# Observabilité des systèmes
Contient l'ensemble des activités réalisées sur le module 'Observabilité des systèmes' sur la partie Alloy

# Grafana Alloy & OpenTelemetry
# 1 : Fondamentaux

## Exercice 1  ·  Mettre Alloy en route

•	Objectif : déployer Alloy dans un namespace Kubernetes via Helm, avec un pipeline OTLP relié à un exporteur debug. Faire du port-forward sur l'UI pour inspecter le graphe.<br>

1) Création d'un namespace nommé 'observability' à l'aide de kubectl.
<img width="695" height="197" alt="image" src="https://github.com/user-attachments/assets/fafb0199-721c-4808-8ad4-5837a151bcb2" />

2) Création d'un fichier values.yaml.
<img width="530" height="822" alt="image" src="https://github.com/user-attachments/assets/b00574a0-1f4f-413c-bea6-aed18ee6fdd6" />


3) Installation de Alloy via Helm.
<img width="823" height="217" alt="image" src="https://github.com/user-attachments/assets/7f08617c-cb1f-4044-9807-11e4c5b8ee33" />

4) 
