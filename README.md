
# 🖥️ CyberMonitor - Supervision Windows Server

Projet de supervision d'un serveur Windows via une interface web développée en HTML/CSS/JS, connecté à une infrastructure locale avec des machines virtuelles en temps reel.

## 🧰 Infrastructure

Le système repose sur **trois machines** :
- 💻 Une **machine physique** (l'hôte) : utilisée pour le développement du dashboard et l'affichage de la supervision.
- 🖥️ Une **machine virtuelle Windows Server** : héberge les services supervisés.
- 🪟 Une **machine virtuelle Windows 10** : utilisée comme client pour accéder au dashboard.

## 🛠️ Logiciels utilisés

- **VMware Workstation Pro** : pour la virtualisation
- **Windows Exporter** : pour l'export des métriques système du serveur
- **Prometheus** : pour collecter les métriques
- **Visual Studio Code** : pour le développement du site web
- **CMD (Invite de commandes)** : pour configuration réseau, tests, etc.

## 🛠️ Étapes du projet

1. **Création et configuration des machines virtuelles**
2. **Installation et configuration des logiciels nécessaires** (Exporter, Prometheus, etc.)
3. **Liaison réseau des machines virtuelles avec la machine physique**
4. **Extraction et envoi des informations système vers la machine hôte via Prometheus**
5. **Développement du dashboard de supervision avec des graphes et une interface interactive**
6. **Connexion du dashboard au serveur Prometheus pour récupérer et afficher les métriques**

## 🔐 Fonctionnalités du site

- **Authentification basique** via `login.html` avec vérification dans `users.json`
- **Tableau de bord** (`dashboardfinal.html`) :
  - Surveillance de la mémoire, CPU, disque, threads
  - Affichage de logs et alertes
  - Console d'administration simulée
  - Section de configuration (seuils, alertes mail, etc.)

## 📁 Structure du projet

```
.
├── login.html              
├── dashboardfinal.html     
├── users.json              
└── /images                 
```

## 📊 Technologies web

- **HTML/CSS/JS**
- **Chart.js** pour les graphes
- **Fetch API** pour la récupération des données Prometheus
- **Font Awesome** pour les icônes

## ▶️ Lancer le projet


### 1. 🖥️ Sur la machine Windows Server (machine virtuelle)

- Télécharger **[Windows Exporter](https://github.com/prometheus-community/windows_exporter)**.
- Lancer l’exécutable ou installer en tant que service :
  ```powershell
  windows_exporter.exe --collectors.enabled "cpu,cs,logical_disk,net,os,system"
  ```
- Le service sera accessible sur le port `9182` par défaut : `http://localhost:9182/metrics`

### 2. 📊 Sur la machine hôte (physique)

#### a. Télécharger et configurer **Prometheus** :

- Télécharger [Prometheus](https://prometheus.io/download/)
- Décompresser et modifier le fichier `prometheus.yml` :
  ```yaml
  scrape_configs:
    - job_name: 'windows-server'
      static_configs:
        - targets: ['IP_DE_LA_VM_SERVEUR:9182']
  ```
  > Remplace `IP_DE_LA_VM_SERVEUR` par l’adresse IP de ta VM Windows Server (ex. `192.168.214.131`)

- Lancer Prometheus :
  ```bash
  prometheus.exe --config.file=prometheus.yml
  ```
- Interface Prometheus disponible sur : `http://localhost:9090`

### 3. 🌐 Lancer le Dashboard Web

- Ouvrir le dossier contenant `login.html`, `dashboardfinal.html` et `users.json`
- Démarrer un serveur local :
  ```bash
  npx serve .
  ```
  ou
  ```bash
  python -m http.server
  ```
- Accéder au site depuis un navigateur à l’adresse :
  ```
  http://localhost:PORT/login.html
  ```
## ✅ Identifiants de test

```json
{
  "Bogdan": "Salagean",
  "Thomas": "Jaillant",
  "Souhaliho": "Bamba"
}
```

## 🚀 Objectif final

Permettre à la **machine physique** (l’hôte) d’accéder et superviser **en temps réel** les données du **serveur Windows** via un **dashboard web personnalisé**.

