#  Infrastructure Monitoring — Prometheus & Grafana

![Prometheus](https://img.shields.io/badge/Prometheus-2.46.0-E6522C?style=for-the-badge&logo=prometheus)
![Grafana](https://img.shields.io/badge/Grafana-12.4-F46800?style=for-the-badge&logo=grafana)
![Ubuntu](https://img.shields.io/badge/Ubuntu-20.04_LTS-E95420?style=for-the-badge&logo=ubuntu)
![Docker](https://img.shields.io/badge/Linux-Monitoring-FCC624?style=for-the-badge&logo=linux)

##  Objectif

Mise en place d'une **stack de monitoring complète** pour la surveillance en temps réel d'un serveur Linux, en utilisant les outils standards de l'industrie DevOps.

##  Architecture
┌─────────────────────────────────────────────────────┐
│ Ubuntu 20.04 LTS │
│ │
│ ┌──────────────┐ ┌─────────────┐ │
│ │ Node Exporter│───▶│ Prometheus │ │
│ │ :9100 │ │ :9090 │ │
│ └──────────────┘ └──────┬──────┘ │
│ │ │
│ ┌──────▼──────┐ │
│ │ Grafana │ │
│ │ :3000 │ │
│ └─────────────┘ │
└─────────────────────────────────────────────────────┘

---

 🛠️ Stack technique

| Outil | Version | Rôle |
|-------|---------|------|
| **Prometheus** | 2.46.0 | Collecte et stockage des métriques |
| **Node Exporter** | 1.6.1 | Exposition des métriques système Linux |
| **Grafana** | 12.4 | Visualisation et tableaux de bord |
| **Ubuntu** | 20.04 LTS | Système d'exploitation |
| **VMware** | Workstation | Virtualisation |



##  Installation

### Prérequis
- Ubuntu 20.04 LTS
- Accès sudo
- Ports : `9090` (Prometheus), `9100` (Node Exporter), `3000` (Grafana)

### 1️ Prometheus

```bash
# Création utilisateur système dédié
sudo useradd --no-create-home --shell /bin/false prometheus
sudo mkdir /etc/prometheus /var/lib/prometheus
sudo chown prometheus:prometheus /var/lib/prometheus

# Téléchargement et installation
cd /tmp/
wget https://github.com/prometheus/prometheus/releases/download/v2.46.0/prometheus-2.46.0.linux-amd64.tar.gz
tar -xvf prometheus-2.46.0.linux-amd64.tar.gz
cd prometheus-2.46.0.linux-amd64
sudo mv console* prometheus.yml /etc/prometheus
sudo mv prometheus /usr/local/bin/
sudo chown -R prometheus:prometheus /etc/prometheus
sudo chown prometheus:prometheus /usr/local/bin/prometheus

# Démarrage du service
sudo systemctl daemon-reload
sudo systemctl enable --now prometheus
```

### 2️ Node Exporter

```bash
cd /tmp
wget https://github.com/prometheus/node_exporter/releases/download/v1.6.1/node_exporter-1.6.1.linux-amd64.tar.gz
sudo tar xvfz node_exporter-*.*-amd64.tar.gz
sudo mv node_exporter-*.*-amd64/node_exporter /usr/local/bin/
sudo useradd -rs /bin/false node_exporter
sudo systemctl enable --now node_exporter
```

### 3️ Grafana

```bash
wget -q -O - https://packages.grafana.com/gpg.key | sudo apt-key add -
sudo add-apt-repository "deb https://packages.grafana.com/oss/deb stable main"
sudo apt update && sudo apt install grafana -y
sudo systemctl enable --now grafana-server
```

---

##  Métriques surveillées

-  **CPU** — Utilisation en temps réel
-  **RAM** — Mémoire utilisée / disponible
-  **Disque** — Espace utilisé / libre
-  **Réseau** — Trafic entrant / sortant
-  **Uptime** — Disponibilité du serveur

---

##  Accès aux services

| Service | URL | Credentials |
|---------|-----|-------------|
| Prometheus | `http://localhost:9090` | — |
| Prometheus Targets | `http://localhost:9090/targets` | — |
| Grafana | `http://localhost:3000` | admin / admin |



##  Dashboard

Dashboard importé depuis **Grafana.com** — ID : `14513`

> Linux Exporter Node — affiche CPU, RAM, Réseau et Disque en temps réel

---

##  Captures d'écran
<img width="370" height="325" alt="image" src="https://github.com/user-attachments/assets/448a6ad2-a300-478d-9213-3d461353a8a7" />


### Prometheus — Targets actives
<img width="338" height="402" alt="image" src="https://github.com/user-attachments/assets/990fb516-7548-47c1-b19c-ad6e13d24390" />


### Grafana — Dashboard Linux Monitoring
<img width="558" height="378" alt="image" src="https://github.com/user-attachments/assets/baa67178-4c3f-4b9e-b55d-66500c194bf3" />


