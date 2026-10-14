# 🛡️ Hybrid Cyber Homelab — Architecture Zero-Trust & Edge Computing

![Status](https://img.shields.io/badge/Status-En_cours_de_déploiement-brightgreen)
![Network](https://img.shields.io/badge/Mesh_VPN-Tailscale_(WireGuard)-blue)
![Hardware](https://img.shields.io/badge/Hardware-Legion_5_Pro_%7C_MagicBook_14_%7C_RPi_4-orange)
![Focus](https://img.shields.io/badge/Focus-Cybersécurité_&_DevSecOps-red)

## 📌 Contexte & Problématique
En tant qu'étudiant en **Bachelor Informatique & Cybersécurité**, j'ai besoin d'une importante puissance de calcul (CPU/RAM/GPU) pour faire tourner des environnements de virtualisation lourds (Labs Active Directory, Kali Linux, SIEM, Docker, Hashcat) tout en conservant une mobilité et une autonomie maximales en cours.

* **Problème :** Un PC portable "Workstation/Gamer" (**Lenovo Legion 5 Pro - RTX 5060**) est trop lourd et énergivore pour être transporté en amphi, tandis qu'un ultraportable (**Honor MagicBook 14**) manque de ressources pour exécuter plusieurs machines virtuelles simultanément.
* **Solution :** Concevoir une **architecture hybride distante Zero-Trust** permettant de déporter la charge de calcul à domicile tout en utilisant l'ultraportable comme terminal léger sécurisé, orchestré par un **Raspberry Pi 4** basse consommation (allumé 24h/24).

---

## 🏗️ Architecture Globale du Projet

```mermaid
graph LR
    subgraph "Campus / Mobilité (Wi-Fi Eduroam / 5G)"
        MB[💻 Honor MagicBook 14<br/>Client Léger - Windows 11]
    end

    subgraph "Réseau Mesh Chiffré (Tailscale / WireGuard)"
        TS((🔒 Tunnel P2P<br/>NAT Traversal))
    end

    subgraph "Domicile (LAN / Fibre)"
        RPI[🍓 Raspberry Pi 4 - 24/7<br/>OS Lite 64-bit + Docker]
        LEGION[🖥️ Lenovo Legion 5 Pro<br/>Serveur de Calcul & Labs VMs]
        
        RPI -->|Magic Packet UDP<br/>Wake-on-LAN| LEGION
    end

    MB <-->|SSH / Parsec / RDP / HTTPS| TS
    TS <-->|100.x.y.z| RPI
    TS <-->|100.x.y.z| LEGION