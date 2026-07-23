# Lab 03 — HSRP : priorité, preempt, tracking

**Semaine 1 · Domaine 3.0 Infrastructure · ~40 min**

## Objectifs
- Passerelle redondante HSRP entre D1 et D2
- Bascule automatique sur panne d'uplink via tracking
- Comprendre le rôle CRITIQUE de preempt

## Topologie
```
      (WAN simulé : loopback ou R1)
        |                |
  g0/1  D1 ---- lien ---- D2  g0/1
        |    (VLAN 10)    |
        +------ A1 -------+
                |
               PC1 (10.0.10.100/24, GW 10.0.10.254)
```

## Tâches
1. SVI VLAN 10 : D1 = 10.0.10.1, D2 = 10.0.10.2.
2. **HSRP** :
   ```
   D1(config-if)# standby version 2
   D1(config-if)# standby 1 ip 10.0.10.254
   D1(config-if)# standby 1 priority 110
   D1(config-if)# standby 1 preempt
   D2 : priority 100 + preempt
   ```
3. `show standby brief` : D1 Active, D2 Standby. Note la vMAC (0000.0c9f.f001 en v2).
4. **Bascule brutale** : ping continu depuis PC1 vers 10.0.10.254 → `shutdown` du SVI de D1 → combien de pings perdus ? D2 devient Active.
5. **Tracking** : rallume D1 (il repasse Active grâce à preempt), puis :
   ```
   D1(config)# track 8 interface g0/1 line-protocol
   D1(config-if)# standby 1 track 8 decrement 30
   ```
   Coupe g0/1 de D1 : priorité 110→80 < 100 → D2 preempt et devient Active **sans** que le lien LAN soit tombé. C'est le scénario d'examen classique.
6. **Test négatif** : retire `preempt` de D2 et refais l'étape 5. Constate que D2 ne prend PAS le relais malgré sa meilleure priorité → preempt est indispensable côté "challenger".

## Vérifications clés
`show standby brief` · `show track` · `debug standby events` (avec modération)

## Défi bonus
Remplace le tracking d'interface par une sonde : `ip sla 1` (icmp-echo vers le WAN) + `track 8 ip sla 1 reachability`. Plus fiable : détecte les pannes AU-DELÀ du lien local.

## Lien examen
Timers 3/10, v1 vs v2 (groupes, multicast, vMAC), la chaîne SLA→track→decrement→preempt, comparaison HSRP/VRRP/GLBP.
