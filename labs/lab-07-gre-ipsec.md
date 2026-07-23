# Lab 07 — GRE + OSPF dans le tunnel, puis IPsec

**Semaine 3 · Domaines 2.0/3.0 · ~60 min**

## Objectifs
- Tunnel GRE R1–R3 par-dessus R2 (qui ignore tout)
- OSPF DANS le tunnel + éviter le routage récursif
- Chiffrer avec un profil IPsec (méthode moderne)

## Topologie
```
R1 ---- 10.0.12.0/30 ---- R2 ---- 10.0.23.0/30 ---- R3
        Tunnel0 : 172.16.13.0/30 (R1 ↔ R3, via R2)
LAN R1 : 192.168.1.0/24          LAN R3 : 192.168.3.0/24
```

## Tâches
1. Routage de base : routes statiques (ou OSPF area 0) pour que R1 et R3 joignent leurs IP WAN mutuelles À TRAVERS R2. R2 ne connaît PAS les LAN.
2. **GRE** :
   ```
   R1(config)# interface Tunnel0
   R1(config-if)# ip address 172.16.13.1 255.255.255.252
   R1(config-if)# tunnel source g0/0
   R1(config-if)# tunnel destination 10.0.23.2
   R1(config-if)# ip mtu 1400
   R1(config-if)# ip tcp adjust-mss 1360
   ```
   Miroir sur R3. Ping 172.16.13.2 : le tunnel vit.
3. **OSPF dans le tunnel** : `ip ospf 10 area 0` sur Tunnel0 et les LAN (PAS sur les interfaces WAN !). Les LAN s'apprennent via le tunnel. Traceroute R1-LAN → R3-LAN : un seul "saut" tunnel (R2 invisible).
4. **Provoquer le routage récursif** (pédagogique) : ajoute les interfaces WAN dans OSPF. Le routeur apprend la destination du tunnel VIA le tunnel → messages `%TUN-5-RECURDOWN`, flapping. Comprends, puis retire.
5. **IPsec par profil** :
   ```
   crypto ikev2 proposal PROP : encryption aes-cbc-256 / integrity sha256 / group 14
   crypto ikev2 policy POL : proposal PROP
   crypto ikev2 keyring KEY : peer R3 / address 10.0.23.2 / pre-shared-key Encor2026!
   crypto ikev2 profile PROF : match identity remote address 10.0.23.2 / authentication local|remote pre-share / keyring local KEY
   crypto ipsec transform-set TS esp-aes 256 esp-sha256-hmac (mode transport)
   crypto ipsec profile IPSECPROF : set transform-set TS / set ikev2-profile PROF
   interface Tunnel0 : tunnel protection ipsec profile IPSECPROF
   ```
6. Vérifie : `show crypto ikev2 sa`, `show crypto ipsec sa` (encaps/decaps qui s'incrémentent pendant un ping).

## Vérifications clés
`show interface tunnel0` · `show ip ospf neighbor` · `show crypto ipsec sa` · ping avec `size 1500 df-bit` (constate le code M → MTU du chemin)

## Lien examen
GRE protocole 47, MTU 1476/1400, transport vs tunnel mode, tunnel protection vs crypto map, recursive routing, ESP=50.
