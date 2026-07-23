# Lab 05 — OSPFv3 : la même topologie en IPv6

**Semaine 2 · Domaine 3.0 Infrastructure · ~40 min**

## Objectifs
- Reproduire le lab 04 en IPv6
- Comprendre les différences structurantes v2/v3

## Topologie
Identique au lab 04. Adressage : 2001:db8:12::/64 (R1–R2), 2001:db8:23::/64 (R2–R3), loopbacks 2001:db8::X/128.

## Tâches
1. `ipv6 unicast-routing` (l'oubli n°1 !).
2. Adresses IPv6 sur les interfaces + link-local automatiques (observe-les : `show ipv6 interface brief`).
3. **OSPFv3 par interface** (pas de network statement) :
   ```
   R1(config-if)# ipv6 ospf 10 area 0
   ```
   Le router-id reste 32 bits : le fixer manuellement (`router-id 1.1.1.1` sous `ipv6 router ospf 10`) car sans IPv4, pas d'élection automatique possible.
4. Vérifie les adjacences : `show ipv6 ospf neighbor` → note que les voisins dialoguent en **link-local** (FE80::...).
5. `show ipv6 route ospf` : codes O / OI.
6. Refais l'aire 1 stub totally stubby comme au lab 04.

## Vérifications clés
`show ipv6 ospf neighbor` · `show ipv6 ospf interface` · `show ipv6 route`

## Défi bonus
Sur IOS-XE récent, teste le mode address-family d'OSPFv3 (`router ospfv3 10` + `address-family ipv4/ipv6 unicast`) : un seul process pour les deux piles.

## Lien examen
Activation par interface, adjacences en link-local, multicast FF02::5/FF02::6, router-id 32 bits obligatoire, différences v2/v3 en tableau.
