# Lab 09 — NAT/PAT + IP SLA : bascule dual-WAN automatique

**Semaine 3 · Domaines 3.0/4.0 · ~45 min**

## Objectifs
- PAT vers l'extérieur (le montage le plus courant au monde)
- Sonde IP SLA + tracking + route flottante = bascule automatique
- Le scénario multi-FAI typique (très concret pour Haïti !)

## Topologie
```
LAN 192.168.50.0/24 --- R1 --- g0/1 --- FAI-1 (203.0.113.1) [principal]
                          \\--- g0/2 --- FAI-2 (198.51.100.1) [secours]
```
(FAI-1/FAI-2 simulés par un routeur avec deux loopbacks "Internet" : 8.8.8.8/32 par exemple.)

## Tâches
1. **PAT** :
   ```
   R1(config)# access-list 1 permit 192.168.50.0 0.0.0.255
   R1(config)# ip nat inside source list 1 interface g0/1 overload
   R1(config)# interface g0/0 → ip nat inside
   R1(config)# interface g0/1 → ip nat outside
   R1(config)# interface g0/2 → ip nat outside
   ```
   ⚠ Vérifie bien inside côté LAN, outside côté WAN — l'inversion est le piège d'exhibit classique.
2. Ping depuis un PC vers 8.8.8.8, puis `show ip nat translations` : observe inside local / inside global et les ports PAT.
3. **Sonde** :
   ```
   ip sla 1
    icmp-echo 8.8.8.8 source-interface g0/1
    frequency 5
   ip sla schedule 1 life forever start-time now
   track 8 ip sla 1 reachability
   ```
4. **Routes** :
   ```
   ip route 0.0.0.0 0.0.0.0 203.0.113.1 track 8
   ip route 0.0.0.0 0.0.0.0 198.51.100.1 250
   ```
5. **Test de bascule** : ping continu depuis le PC → coupe le lien FAI-1 (ou filtre l'ICMP de la sonde) → `show track` passe Down → la route principale disparaît → la flottante (AD 250) prend le relais. Combien de secondes de coupure ?
6. **PAT sur la 2e sortie** : ajoute la règle overload sur g0/2 (avec route-map pour choisir l'interface selon la sortie — bonus avancé).

## Vérifications clés
`show ip nat translations` · `show ip nat statistics` · `show ip sla statistics` · `show track` · `show ip route | include 0.0.0.0`

## Lien examen
Terminologie NAT (inside/outside, local/global), PAT/overload, ordre NAT vs ACL, IP SLA + track + floating static (question quasi garantie), `ip sla responder` pour udp-jitter.
