# Lab 06 — eBGP : session, annonces et best-path

**Semaine 2 · Domaine 3.0 Infrastructure · ~45 min**

## Objectifs
- Monter une session eBGP et annoncer des réseaux
- Lire `show ip bgp` (codes >, *, i)
- Comprendre le piège du network à correspondance exacte

## Topologie
```
R1 (AS 65001) ---- 10.99.12.0/30 ---- R2 (AS 65002)
Lo0 R1 : 192.168.1.1/24 annoncé      Lo0 R2 : 192.168.2.1/24 annoncé
```

## Tâches
1. **Session** :
   ```
   R1(config)# router bgp 65001
   R1(config-router)# neighbor 10.99.12.2 remote-as 65002
   ```
   Miroir sur R2. `show ip bgp summary` : l'état doit passer Idle → Active → OpenSent → Established (colonne State/PfxRcd = un nombre).
2. **Annonce** :
   ```
   R1(config-router)# network 192.168.1.0 mask 255.255.255.0
   ```
3. **LE piège** : annonce `network 192.168.1.0 mask 255.255.0.0` (masque volontairement faux). Constate : rien ne part (`show ip bgp` vide côté R2 pour ce préfixe). BGP exige la correspondance EXACTE préfixe+masque dans la table de routage. Corrige.
4. **Lecture** : `show ip bgp` sur R2 :
   - `*` = route valide, `>` = best path, Next Hop, AS Path `65001 i`.
5. **Diagnostic Idle** : `shutdown` sur l'interface WAN de R2, `clear ip bgp *` sur R1 → session Idle. `show ip route 10.99.12.2` = network not in table → BGP ne peut même pas tenter le TCP 179. Rallume et regarde la machine à états remonter.
6. **Loopback peering (bonus multihop)** : monte la session entre Lo0 avec `update-source Loopback0` + `ebgp-multihop 2` + routes statiques croisées. Comprends pourquoi les DEUX commandes sont nécessaires.

## Vérifications clés
`show ip bgp summary` · `show ip bgp` · `show ip bgp neighbors | include BGP state` · `show tcp brief`

## Lien examen
États BGP dans l'ordre, TTL eBGP=1, update-source/multihop, network mask exact, attributs (weight > local-pref > AS-path...), timers 60/180.
