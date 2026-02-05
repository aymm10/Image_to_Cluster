# 🧪 Atelier (évaluation)

## 🎯 Objectif de l’atelier

Cet atelier a pour but de **démontrer l’industrialisation du cycle de vie d’une application** simple, depuis la construction d’une image applicative jusqu’à son déploiement automatisé sur un cluster Kubernetes.

Nous utilisons pour cela :

- **GitHub Codespaces** : environnement de travail reproductible
- **Packer** : création d’une image Docker customisée
- **Nginx** : serveur web servant une page statique
- **K3d (K3s + Docker)** : cluster Kubernetes léger
- **Ansible** : déploiement automatisé sur Kubernetes
- **Makefile** : orchestration complète du workflow

👉 L’objectif final est de **passer d’un artefact applicatif maîtrisé à un déploiement automatisé sur Kubernetes**, en s’appuyant sur des outils d’Infrastructure as Code.

---

## 🏗️ Architecture cible

L’architecture finale est la suivante :

![Screenshot Actions](Architecture_cible.png)

---

## 🧰 Prérequis

Aucun prérequis local n’est nécessaire.

Il suffit de disposer de :
- Un **compte GitHub**
- L’accès à **GitHub Codespaces**

Tout le reste (Docker, Packer, Ansible, kubectl, k3d…) est installé automatiquement.

---

## 🚀 Démarrage rapide

### 1️⃣ Fork du projet

- Forkez ce repository GitHub
- Depuis l’onglet **CODE**, cliquez sur **Open with Codespaces**
- Attendez que l’environnement soit prêt

---

## ⚙️ Automatisation via Makefile

L’intégralité du projet est orchestrée à l’aide d’un **Makefile**.

### Commande principale

```bash
make all
```
Cette commande exécute automatiquement :
  - L’installation des dépendances
  - La création de l’image Docker avec Packer
  - La création du cluster K3d
  - L’import de l’image dans le cluster
  - Le déploiement Kubernetes via Ansible
  - Le port-forward pour accéder à l’application

---

### 🌍 Accès à l’application

Un port-forward est lancé automatiquement :

```text
localhost:8081 → Service Kubernetes (port 80)
```

👉 Dans GitHub Codespaces :
  - Allez dans l’onglet PORTS
  - Cherchez le port "8081"
  - Ouvrez l’URL dans votre navigateur

🎉 Vous devriez voir la page index.html servie par Nginx !
