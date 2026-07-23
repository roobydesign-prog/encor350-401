# Lab 04 — OSPF multi-aires : LSA, stub, résumé

**Semaine 2 · Domaine 3.0 Infrastructure · ~60 min**

## Objectifs
- Monter une topologie aire 0 + aire 1
- Lire la LSDB et identifier chaque type de LSA
- Configurer une aire stub totally stubby + résumé inter-aires

## Topologie
```
R3 ---- Aire 1 ---- R2(ABR) ---- Aire 0 ---- R1
Lo0: 10.1.3.1/32              Lo0: 10.1.1.1/32
+ 172.16.0.0/24 à 172.16.3.0/24 (loopbacks sur R3)
```

## Tâches
1. OSPF process 10 partout, router-id manuels (1.1.1.1 / 2.2.2.2 / 3.3.3.3).
2. Active OSPF par interface (méthode moderne) : `ip ospf 10 area 0` (ou area 1 côté R3).
3. **Lecture LSDB** sur R2 : `show ip ospf database`. Identifie : Router LSA (type 1) par aire, Network LSA (type 2) si segment multi-accès, Summary LSA (type 3) générées par R2 vers chaque aire.
4. **Codes de routes** sur R1 : `show ip route ospf` → les réseaux de l'aire 1 apparaissent en `O IA`.
5. **Stub** : passe l'aire 1 en totally stubby :
   ```
   R3(config-router)# area 1 stub
   R2(config-router)# area 1 stub no-summary
   ```
   Sur R3 : `show ip route ospf` → tout a disparu sauf `O*IA 0.0.0.0/0`. C'est le but : table minimale.
6. **Résumé** : sur R2, agrège les loopbacks de R3 :
   ```
   R2(config-router)# area 1 range 172.16.0.0 255.255.252.0
   ```
   R1 ne voit plus qu'une seule route /22 au lieu de quatre /24.
7. **Réglages d'interface** : sur le lien R1–R2, `ip ospf network point-to-point` → plus d'élection DR inutile. Compare `show ip ospf interface` avant/après.

## Vérifications clés
`show ip ospf neighbor` · `show ip ospf database` · `show ip ospf interface brief` · `show ip route ospf`

## Défi bonus
Ajoute une route statique vers un réseau fictif sur R1 et redistribue-la (`redistribute static subnets`). Observe le type 5 dans la LSDB, le code `O E2` sur R3... impossible ! (aire stub = pas de type 5). Repasse l'aire 1 en normale pour le voir, puis en NSSA pour observer le type 7 et le code `O N2`.

## Lien examen
Types de LSA 1/2/3/4/5/7, règles stub/totally stubby/NSSA (qui configure quoi), E1 vs E2, network types et timers, virtual-link.
