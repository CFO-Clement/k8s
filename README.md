```
kubernetes-deployment/
│
├── inventory
│   └── hosts.ini
│
├── playbooks/
│   ├── setup-docker.yml
│   ├── setup-kubernetes.yml
│   └── deploy-application.yml
│
└── roles/
    ├── docker/
    │   └── tasks/
    │       └── main.yml
    ├── kubernetes/
    │   └── tasks/
    │       └── main.yml
    └── application/
        └── tasks/
            └── main.yml
```
## Inventory
- `hosts.ini` : inventaire des targets

## Playbooks
- `setup-docker.yml` : Installe les dependance
- `setup-kubernetes.yml` : Configure Minikube
- `deploy-application.yml` : Déploie votre l'application

# Utilisation
- `ansible-playbook -i inventory/hosts.ini playbooks/setup-docker.yml` #Install dependance
- `ansible-playbook -i inventory/hosts.ini playbooks/setup-kubernetes.yml` #Config k8s
- `ansible-playbook -i inventory/hosts.ini playbooks/deploy-application.yml` #Deploy app

# Notes
- https://maxat-akbanov.com/prometheus-and-grafana-setup-in-minikube
    - Exemple de template:
        - 6417
        - 15759

## Playbook: setup-docker
Ce playbook Ansible configure une machine Linux pour la gestion de conteneurs et l'orchestration Kubernetes en effectuant les tâches suivantes :

1. **Installation des paquets nécessaires** : Installe des paquets pour la sécurité, la gestion de paquets, Docker, et les opérations réseau.

2. **Configuration de Docker** : Assure que Docker est démarré et activé au démarrage de la machine.

3. **Installation de Minikube** : Télécharge et installe Minikube, un outil qui permet de lancer un cluster Kubernetes localement.

4. **Installation de kubectl** : Installe kubectl, l'outil en ligne de commande pour interagir avec des clusters Kubernetes.

5. **Installation et configuration de Helm** : Télécharge Helm, un gestionnaire de paquets pour Kubernetes, le décompresse et déplace les binaires nécessaires.

6. **Installation de Kompose** : Télécharge et installe Kompose, un outil pour convertir des fichiers Docker Compose en ressources Kubernetes.

## Playbook: setup-kubernetes
Ce playbook Ansible automatise la configuration et la mise en place d'un environnement de monitoring pour Kubernetes utilisant Minikube, Prometheus, et Grafana :

1. **Démarrer Minikube** : Lance Minikube en utilisant Docker comme driver pour créer un cluster Kubernetes local.

2. **Créer un namespace monitoring** : Crée un espace de nommage spécifique (`monitoring`) dans Kubernetes pour isoler les ressources de monitoring.

3. **Configurer Helm pour Prometheus et Grafana** : Ajoute les dépôts Helm de Prometheus et Grafana, puis met à jour les dépôts pour s'assurer que les dernières versions des charts sont disponibles.

4. **Déployer Prometheus** : Installe Prometheus dans le namespace `monitoring` à l'aide de Helm.

5. **Exposer Prometheus** : Crée un service Kubernetes pour rendre Prometheus accessible via un port spécifique sur le cluster.

6. **Attendre le démarrage de Prometheus** : Pause de 30 secondes pour permettre à Prometheus de se lancer.

7. **Lister les services Minikube** : Liste les services en cours dans Minikube, incluant les URL pour accéder aux services exposés.

8. **Obtenir l'URL de Prometheus** : Récupère l'URL pour accéder à Prometheus exposé, et l'enregistre pour utilisation ultérieure.

9. **Déployer Grafana** : Installe Grafana dans le namespace `monitoring` en utilisant Helm.

10. **Exposer Grafana** : Crée un service pour rendre Grafana accessible via un port spécifique sur le cluster.

11. **Récupérer les identifiants de Grafana** : Extrait le mot de passe administrateur de Grafana stocké dans les secrets Kubernetes et le décode.

12. **Obtenir l'URL de Grafana** : Récupère l'URL pour accéder à Grafana exposé, et l'enregistre pour utilisation ultérieure.

13. **Créer un namespace application** : Crée un autre namespace pour une utilisation séparée, probablement pour des applications spécifiques à déployer séparément.

14. **Afficher les identifiants de Grafana et les URL d'accès** : Affiche le mot de passe de l'administrateur de Grafana et les URLs pour Grafana et Prometheus pour faciliter l'accès.

Ce playbook est essentiel pour la mise en place d'un environnement de surveillance robuste en utilisant des outils modernes et populaires dans l'écosystème Kubernetes.

## Playbook: deploy_application:
Ce playbook Ansible automatisera le déploiement d'une application Angular en utilisant Docker et Kubernetes, à travers les étapes suivantes :

1. **Cloner un dépôt GitHub** : Clone le dépôt spécifié dans un répertoire local et assure que le dépôt local est à jour avec la version en ligne.

2. **Construire des images Docker** : Utilise Docker pour construire une image de l'application Angular depuis les fichiers situés dans le projet cloné.

3. **Charger l'image dans Minikube** : Charge l'image Docker construite dans l'environnement Minikube, permettant à Minikube de l'utiliser pour déployer des conteneurs.

4. **Convertir docker-compose en configurations Kubernetes** : Utilise Kompose pour transformer un fichier `docker-compose.yml` en configurations Kubernetes nécessaires pour le déploiement sur Minikube.

5. **Modifier la politique de téléchargement d'image** : Ajuste les fichiers de configuration Kubernetes pour s'assurer que la politique de téléchargement de l'image (`imagePullPolicy`) est réglée sur `IfNotPresent`, ce qui empêche Kubernetes de chercher une image sur un registre distant si celle-ci est déjà présente localement.

6. **Appliquer les configurations Kubernetes** : Déploie l'application en appliquant les configurations Kubernetes générées dans l'espace de nommage `application`.

7. **Exposer l'application** : Crée un service Kubernetes pour rendre l'application accessible via un port spécifié, utilisant le type de service `NodePort` qui permet d'accéder à l'application de l'extérieur du cluster Minikube.

Ce playbook assure un déploiement fluide et automatisé d'une application web, de la source au service, dans un environnement Minikube.