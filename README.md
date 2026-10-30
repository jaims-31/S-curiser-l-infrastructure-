# Bilan C3 - Sécuriser l'infrastructure

## 1. Situation initiale
Le cluster AKS `aks-observability` a été démarré avec la configuration de départ :
- Local RBAC activé
- Azure Monitor désactivé
- Cluster public accessible sans restriction IP

## 2. Whitelist d'IP
Pour restreindre l'accès au cluster, nous avons ajouté une règle d'autorisation IP. Seule l'adresse IP des locaux de Simplon (`86.201.70.133`) est autorisée à se connecter au control plane du cluster.

Commande utilisée :
```bash
az aks update -g fbarryRG -n aks-observability --api-server-authorized-ip-ranges 86.201.70.133/32
