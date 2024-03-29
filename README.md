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
- `hosts.ini` : Fichier d'inventaire pour votre VM.

## Playbooks
- `setup-docker.yml` : Installe Docker sur la VM.
- `setup-kubernetes.yml` : Installe kubeadm, kubelet, kubectl et initialise le cluster.
- `deploy-application.yml` : Déploie votre application Django sur le cluster.

## Roles
- Le rôle `docker/tasks/main.yml` installe Docker.
- Le rôle `kubernetes/tasks/main.yml` installe kubeadm, kubelet, kubectl et initialise le cluster.
- Le rôle `application/tasks/main.yml` contiendrait les tâches pour conteneuriser votre application Django et la déployer (construction de l'image Docker, push vers un registre, création des ressources Kubernetes nécessaires, etc.). Cette partie serait spécifique à votre application.

# Utilisation
- `ansible-playbook -i inventory/hosts.ini playbooks/setup-docker.yml` 
- `ansible-playbook -i inventory/hosts.ini playbooks/setup-kubernetes.yml`
- `ansible-playbook -i inventory/hosts.ini playbooks/deploy-application.yml`

# Notes
- https://maxat-akbanov.com/prometheus-and-grafana-setup-in-minikube

## Playbook: setup-docker
Ce playbook gere l'installation de toutes les dependance et install les binaire necessaire

## Playbook: setup-kubernetes
Ce playbook vas configuere k8s. Il ce base sur MiniKube
il vas cree un namespace monitoring 