# Lab 01 — Spanning Tree : root, convergence et protections

**Semaine 1 · Domaine 3.0 Infrastructure · CML-Free ou Packet Tracer · ~45 min**

## Objectifs
- Forcer l'élection du root bridge et lire la topologie STP
- Chronométrer la convergence RSTP après une coupure
- Sécuriser l'accès avec PortFast + BPDU Guard

## Topologie
```
        D1 ================= D2
         \\    (2 liens)     /
          \\                /
           +----- A1 -----+
                  |
                 PC1 (VLAN 10)
```
3 switches : D1, D2 (distribution), A1 (accès). Deux liens D1–D2, un lien vers A1 depuis chaque distribution. VLAN 10 partout.

## Tâches
1. **Base** : créer VLAN 10, mettre tous les liens inter-switches en trunk statique (`switchport mode trunk`), PC1 en accès VLAN 10.
2. **Observer l'élection par défaut** : `show spanning-tree vlan 10` sur les 3 switches. Qui est root ? Pourquoi (priorité ? MAC ?) Note le port bloqué.
3. **Forcer D1 root** :
   ```
   D1(config)# spanning-tree vlan 10 root primary
   D2(config)# spanning-tree vlan 10 root secondary
   ```
   Vérifie la nouvelle priorité (24576) et le déplacement du port bloqué.
4. **Convergence** : lance un ping continu PC1 → SVI de D1. Coupe le lien actif A1–D1 (`shutdown`). Combien de pings perdus en RSTP (`spanning-tree mode rapid-pvst`) ? Compare avec `spanning-tree mode pvst` (802.1D) : la différence doit être spectaculaire (≈ 1-2 s vs 30-50 s).
5. **Protections d'accès** :
   ```
   A1(config-if)# spanning-tree portfast
   A1(config-if)# spanning-tree bpduguard enable
   ```
   Simule un BPDU entrant sur le port de PC1 (branche un switch à la place). Constate l'err-disable : `show interfaces status err-disabled`. Récupération : `errdisable recovery cause bpduguard`.

## Vérifications clés
`show spanning-tree vlan 10` · `show spanning-tree summary` · `show interfaces trunk` · `show interfaces status err-disabled`

## Défi bonus
Configure MST : les VLAN 10-20 dans l'instance 1, root D1 ; VLAN 30-40 dans l'instance 2, root D2. Vérifie que nom/révision/mapping sont IDENTIQUES sur les 3 switches (`show spanning-tree mst configuration`).

## Lien examen
Flags des états, timers 2/15/20 s, `root primary` = 24576, BPDU Guard vs BPDU Filter, Loop Guard vs UDLD.
