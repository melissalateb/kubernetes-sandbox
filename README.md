# Kubernetes Exercices

Ce dépôt contient des exercices pratiques pour apprendre et expérimenter avec Kubernetes. Les exercices sont conçus pour simuler un pattern controller/agent avec deux serveurs :

- **Planner** : Gère les tâches à exécuter.
- **Worker** : Exécute les tâches.

Dépôt git des exercices : https://github.com/arthurescriou/k8s-exercice/ 

## Prérequis

Avant de commencer, assurez-vous d'avoir installé les outils suivants :

- [Minikube](https://kubernetes.io/fr/docs/tasks/tools/install-minikube/)
- [kubectl](https://kubernetes.io/docs/tasks/tools/)

Assurez-vous également que tout fonctionne correctement en suivant le tutoriel : [Hello Minikube](https://kubernetes.io/docs/tutorials/hello-minikube/).

## Tâches

Les exercices sont axés sur la multiplication et l'addition, avec des workers qui simulent un temps de travail. Voici un résume des tâches réalisées lors de ce projet :

1. **Déployer un pod** : Lancer une exécution de 4 tâches avec un planner et un worker dans un pod.
   
2. **Plusieurs workers** : Permettre le déploiement de plusieurs workers pour un seul planner, afin de paralléliser l'exécution.
   
3. **Spécialisation des workers** : Configurer des workers spécialisés dans une seule tâche et observer leur comportement.

4. **Nombre de worker dynamique** : Ajouter dynamiquement des workers pendant l'exécution du planner et les enregistrer auprès de ce dernier.

5. **Mélanger tout** : Créer une configuration qui exécute une centaine de tâches, dispatchées parmi des workers généralistes et spécialisés.
