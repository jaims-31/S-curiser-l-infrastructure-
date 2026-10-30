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

## 3. Authentification Entra ID & Azure RBAC
Nous avons activé l'intégration Entra ID et le RBAC Azure sur le cluster AKS pour sécuriser et gérer l'accès des utilisateurs via des rôles Azure.

Commande utilisée :
```bash
az aks update -g fbarryRG -n aks-observability --enable-aad
az aks update -g fbarryRG -n aks-observability --enable-azure-rbac

## 4. Gestion des accès RBAC
Nous avons configuré les rôles d'accès sur le cluster :
- Administrateur du cluster attribué à ton compte utilisateur.
- Rôle de lecteur (`Azure Kubernetes Service Cluster User Role`) attribué au formateur (Steve Theval).

## 5. Désactivation des comptes locaux
Pour renforcer la sécurité du cluster et forcer l'authentification unique par Entra ID, nous avons désactivé les comptes locaux administrateurs par défaut.

Commande utilisée :
```bash
az aks update -g fbarryRG -n aks-observability --disable-local-accounts

## 6. Activation d'Azure Policy
Nous avons activé l'addon Azure Policy sur le cluster pour appliquer et vérifier les règles de gouvernance et de sécurité.

Commande utilisée :
```bash
az aks enable-addons -g fbarryRG -n aks-observability --addons azure-policy

## 7. Rôles Azure RBAC et Validation de l'Accès
- Attribution du rôle **Azure Kubernetes Service RBAC Cluster Admin** à l'administrateur.
- Attribution du rôle **Azure Kubernetes Service RBAC Reader** au formateur (Steve Theval).
- Validation de la connexion réussie via `kubectl get nodes`.
