# Lab 02 — EtherChannel : LACP et diagnostic de mismatch

**Semaine 1 · Domaine 3.0 Infrastructure · ~30 min**

## Objectifs
- Monter un LACP L2 entre D1 et D2
- Provoquer et diagnostiquer un mismatch (le flag qui trahit tout)
- Monter un EtherChannel L3

## Topologie
```
D1 ==(g1/0/1 + g1/0/2)== D2
```

## Tâches
1. **LACP L2** :
   ```
   D1(config)# interface range g1/0/1-2
   D1(config-if-range)# channel-group 1 mode active
   D1(config)# interface port-channel 1
   D1(config-if)# switchport mode trunk
   ```
   Idem sur D2 (`mode active` des deux côtés = OK).
2. **Lire les flags** : `show etherchannel summary` → attendu `Po1(SU)` avec `(P)` sur chaque membre.
3. **Provoquer un mismatch** : passe g1/0/2 de D2 en `switchport access vlan 99`. Observe : le port passe en `(s)` suspended. Remets la config, vérifie le retour en `(P)`.
4. **Casser la négociation** : passe D2 en `mode on` pendant que D1 reste `active`. Résultat ? (`(I)` standalone ou down — LACP ne négocie pas avec du statique).
5. **EtherChannel L3** : supprime le Po1, puis :
   ```
   D1(config)# interface range g1/0/1-2
   D1(config-if-range)# no switchport
   D1(config-if-range)# channel-group 2 mode active
   D1(config)# interface port-channel 2
   D1(config-if)# ip address 10.1.12.1 255.255.255.252
   ```
   Ping D2 à travers le bundle L3.
6. **Load-balancing** : `show etherchannel load-balance`, puis `port-channel load-balance src-dst-ip`. Comprends pourquoi src-mac serait mauvais derrière un routeur.

## Vérifications clés
`show etherchannel summary` (flags SU/P/s/I/D) · `show lacp neighbor` · `show etherchannel load-balance`

## Lien examen
active/passive/on (combinaisons qui marchent), conditions d'uniformité des membres, min-links, lecture des flags en exhibit.
