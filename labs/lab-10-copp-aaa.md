# Lab 10 — CoPP + AAA : protéger le plan de contrôle et les accès

**Semaine 3-4 · Domaine 5.0 Sécurité · ~50 min**

## Objectifs
- Policer l'ICMP et protéger SSH vers le control-plane (CoPP)
- AAA avec fallback local (sans s'enfermer dehors !)
- Durcissement de base des lignes

## Topologie
Un routeur R1 + un PC d'administration (10.0.99.10) + un PC "attaquant".

## Tâches
1. **Durcissement de base** :
   ```
   hostname R1 / ip domain name encor.lab
   crypto key generate rsa modulus 2048
   ip ssh version 2
   username admin privilege 15 algorithm-type scrypt secret Encor2026!
   line vty 0 4 : transport input ssh / login local
   access-list 99 permit host 10.0.99.10
   line vty 0 4 : access-class 99 in     ← access-class, PAS access-group !
   login block-for 120 attempts 3 within 60
   ```
2. **Test** : SSH depuis le PC admin = OK ; depuis l'attaquant = refusé (ACL) ; 3 mauvais mots de passe = quiet period.
3. **CoPP** :
   ```
   ip access-list extended ACL-ICMP : permit icmp any any
   class-map CM-ICMP : match access-group name ACL-ICMP
   policy-map COPP
    class CM-ICMP : police 32000 conform-action transmit exceed-action drop
    class class-default : police 1000000 conform-action transmit exceed-action transmit
   control-plane : service-policy input COPP
   ```
   (class-default en transmit/transmit d'abord = mode observation prudent.)
4. **Test CoPP** : flood de pings vers R1 depuis l'attaquant (`ping ... repeat 10000 timeout 0`) → `show policy-map control-plane` : compteurs conformed/exceeded qui montent. Le CPU reste calme (`show processes cpu sorted`).
5. **AAA fallback** (même sans serveur TACACS+ réel, la logique se configure) :
   ```
   aaa new-model
   aaa authentication login VTY-AUTH group tacacs+ local
   line vty 0 4 : login authentication VTY-AUTH
   ```
   Serveur absent → le fallback `local` prend le relais : tu rentres quand même. Ne JAMAIS appliquer une liste sans fallback testé sur la console.

## Vérifications clés
`show policy-map control-plane` · `show ssh` · `show login` · `test aaa` (si serveur disponible)

## Lien examen
CoPP par classes, MPP, iACL, access-class vs access-group, login block-for, listes AAA nommées vs default, TACACS+ vs RADIUS, type 9 scrypt.
