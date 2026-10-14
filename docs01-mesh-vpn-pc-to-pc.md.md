# Phase 1 : Interconnexion P2P des postes de travail & Déport de calcul

⬅️ [Retour au README principal](../README.md)

## 🎯 Objectifs de la Phase 1
1. Établir un tunnel chiffré pair-à-pair (Mesh VPN) entre l'ultraportable utilisé en cours (**Honor MagicBook 14**) et la station de calcul restée à domicile (**Lenovo Legion 5 Pro**).
2. Traverser les pare-feu restrictifs (NAT universitaire Eduroam / 4G / 5G) sans ouvrir aucun port sur le routeur domestique.
3. Configurer le déport d'affichage haute performance (Parsec / RDP) et le développement à distance (VS Code Remote - SSH).

---

## 1. Déploiement du Mesh VPN (Tailscale / WireGuard)

### Topologie des nœuds
Les machines sont enregistrées sur un réseau privé virtuel (*Tailnet*) utilisant la plage d'adressage réservée CGNAT (`100.64.0.0/10`) :

| Nom d'hôte (Machine) | Rôle dans l'architecture | OS | Adresse IP Tailscale |
| :--- | :--- | :--- | :--- |
| `gab-brt` (Honor MagicBook 14) | Client léger mobile (Campus ESILV) | Windows 11 | `100.76.x.x` |
| `legion-gab-brt` (Legion 5 Pro) | Serveur de calcul / Hyperviseur Labs | Windows 11 | `100.87.x.x` |

### Vérification de la connectivité directe (P2P vs DERP)
Pour s'assurer que le trafic ne passe pas par un serveur relais mais bien en pair-à-pair direct (UDP Hole Punching) :
```powershell
tailscale status
tailscale ping legion-gab-brt