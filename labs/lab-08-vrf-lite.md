# Lab 08 — VRF-lite : deux clients isolés sur un routeur

**Semaine 3 · Domaine 2.0 Virtualisation · ~35 min**

## Objectifs
- Créer deux VRF étanches sur le même équipement
- Vérifier l'isolement et manier les commandes "vrf-aware"

## Topologie
```
PC-A (client ROUGE) --- g0/0 [VRF ROUGE] R1 [VRF BLEU] g0/1 --- PC-B (client BLEU)
                        192.168.10.1/24        192.168.10.1/24  ← MÊME adresse !
```

## Tâches
1. **VRF** :
   ```
   R1(config)# vrf definition ROUGE
   R1(config-vrf)# address-family ipv4
   (idem BLEU)
   ```
2. **Affectation** (⚠ l'ordre : le vrf forwarding EFFACE l'IP) :
   ```
   R1(config)# interface g0/0
   R1(config-if)# vrf forwarding ROUGE
   R1(config-if)# ip address 192.168.10.1 255.255.255.0
   ```
   Même IP dans BLEU sur g0/1 → aucune erreur : les tables sont séparées. C'est LA démonstration de la VRF.
3. **Tables** : `show ip route vrf ROUGE` vs `show ip route vrf BLEU` vs `show ip route` (globale, vide de ces réseaux).
4. **Ping vrf-aware** : `ping vrf ROUGE 192.168.10.10` fonctionne ; `ping 192.168.10.10` (global) échoue. Réflexe à ancrer.
5. **Isolement** : PC-A ne peut pas joindre PC-B — aucune fuite entre VRF sans configuration explicite.

## Défi bonus
Route leaking statique : rends un serveur "partagé" du VRF BLEU joignable depuis ROUGE (`ip route vrf ROUGE <prefix> <mask> g0/1 <next-hop>` + retour). Mesure la portée (et le danger) du leaking.

## Lien examen
vrf definition + address-family, vrf forwarding qui efface l'IP, commandes show/ping/traceroute vrf, VRF-lite vs MPLS, cas d'usage (multi-tenant, séparation invités/corporate, underlay/overlay SD-Access).
