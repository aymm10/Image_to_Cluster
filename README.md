------------------------------------------------------------------------------------------------------
ATELIER FROM IMAGE TO CLUSTER
------------------------------------------------------------------------------------------------------
L’idée en 30 secondes : Cet atelier consiste à **industrialiser le cycle de vie d’une application** simple en construisant une **image applicative Nginx** personnalisée avec **Packer**, puis en déployant automatiquement cette application sur un **cluster Kubernetes** léger (K3d) à l’aide d’**Ansible**, le tout dans un environnement reproductible via **GitHub Codespaces**.
L’objectif est de comprendre comment des outils d’Infrastructure as Code permettent de passer d’un artefact applicatif maîtrisé à un déploiement cohérent et automatisé sur une plateforme d’exécution.
  
-------------------------------------------------------------------------------------------------------
Séquence 1 : Codespace de Github
-------------------------------------------------------------------------------------------------------
Objectif : Création d'un Codespace Github  
Difficulté : Très facile (~5 minutes)
-------------------------------------------------------------------------------------------------------
**Faites un Fork de ce projet**. Si besion, voici une vidéo d'accompagnement pour vous aider dans les "Forks" : [Forker ce projet](https://youtu.be/p33-7XQ29zQ) 
  
Ensuite depuis l'onglet [CODE] de votre nouveau Repository, **ouvrez un Codespace Github**.
  
---------------------------------------------------
Séquence 2 : Création du cluster Kubernetes K3d
---------------------------------------------------
Objectif : Créer votre cluster Kubernetes K3d  
Difficulté : Simple (~5 minutes)
---------------------------------------------------
Vous allez dans cette séquence mettre en place un cluster Kubernetes K3d contenant un master et 2 workers.  
Dans le terminal du Codespace copier/coller les codes ci-dessous etape par étape :  

**Création du cluster K3d**  
```
curl -s https://raw.githubusercontent.com/k3d-io/k3d/main/install.sh | bash
```
```
k3d cluster create lab \
  --servers 1 \
  --agents 2
```
**vérification du cluster**  
```
kubectl get nodes
```
**Déploiement d'une application (Docker Mario)**  
```
kubectl create deployment mario --image=sevenajay/mario
kubectl expose deployment mario --type=NodePort --port=80
kubectl get svc
```
**Forward du port 80**  
```
kubectl port-forward svc/mario 8080:80 >/tmp/mario.log 2>&1 &
```
**Réccupération de l'URL de l'application Mario** 
Votre application Mario est déployée sur le cluster K3d. Pour obtenir votre URL cliquez sur l'onglet **[PORTS]** dans votre Codespace et rendez public votre port **8080** (Visibilité du port).
Ouvrez l'URL dans votre navigateur et jouer !

---------------------------------------------------
Séquence 3 : Exercice
---------------------------------------------------
Objectif : Customisez un image Docker avec Packer et déploiement sur K3d via Ansible
Difficulté : Moyen/Difficile (~2h)
---------------------------------------------------  
Votre mission (si vous l'acceptez) : Créez une **image applicative customisée à l'aide de Packer** (Image de base Nginx embarquant le fichier index.html présent à la racine de ce Repository), puis déployer cette image customisée sur votre **cluster K3d** via **Ansible**, le tout toujours dans **GitHub Codespace**.  

**Architecture cible :** Ci-dessous, l'architecture cible souhaitée.   
  
![Screenshot Actions](Architecture_cible.png)   
  
---------------------------------------------------  
## Processus de travail (résumé)

1. Installation du cluster Kubernetes K3d (Séquence 1)
2. Installation de Packer et Ansible
3. Build de l'image customisée (Nginx + index.html)
4. Import de l'image dans K3d
5. Déploiement du service dans K3d via Ansible
6. Ouverture des ports et vérification du fonctionnement

---------------------------------------------------
Séquence 4 : Documentation  
Difficulté : Facile (~30 minutes)
---------------------------------------------------
**Complétez et documentez ce fichier README.md** pour nous expliquer comment utiliser votre solution.  
Faites preuve de pédagogie et soyez clair dans vos expliquations et processus de travail.  
   
---------------------------------------------------
Evaluation
---------------------------------------------------
Cet atelier, **noté sur 20 points**, est évalué sur la base du barème suivant :  
- Repository exécutable sans erreur majeure (4 points)
- Fonctionnement conforme au scénario annoncé (4 points)
- Degré d'automatisation du projet (utilisation de Makefile ? script ? ...) (4 points)
- Qualité du Readme (lisibilité, erreur, ...) (4 points)
- Processus travail (quantité de commits, cohérence globale, interventions externes, ...) (4 points) 


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
