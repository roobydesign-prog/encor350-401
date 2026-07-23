# 🧪 Labs pratiques ENCOR

10 labs progressifs alignés sur le calendrier de 5 semaines, réalisables en **CML-Free** (5 nœuds — recommandé, IOS-XE réel) ou **Packet Tracer** (labs 1-6, 8-9 ; le lab 7 IPsec/IKEv2 et le lab 10 CoPP nécessitent CML).

| # | Lab | Semaine | Domaines |
|---|-----|---------|----------|
| 01 | [STP : root, convergence, protections](lab-01-stp.md) | S1 | 3.0 |
| 02 | [EtherChannel LACP + mismatch](lab-02-etherchannel.md) | S1 | 3.0 |
| 03 | [HSRP : priorité, preempt, tracking](lab-03-hsrp.md) | S1 | 3.0 |
| 04 | [OSPF multi-aires : LSA, stub, résumé](lab-04-ospf-multiaires.md) | S2 | 3.0 |
| 05 | [OSPFv3 en IPv6](lab-05-ospfv3.md) | S2 | 3.0 |
| 06 | [eBGP : session, annonces, best-path](lab-06-ebgp.md) | S2 | 3.0 |
| 07 | [GRE + OSPF + IPsec](lab-07-gre-ipsec.md) | S3 | 2.0/3.0 |
| 08 | [VRF-lite : isolation multi-clients](lab-08-vrf-lite.md) | S3 | 2.0 |
| 09 | [NAT/PAT + IP SLA dual-WAN](lab-09-nat-ipsla.md) | S3 | 3.0/4.0 |
| 10 | [CoPP + AAA + durcissement](lab-10-copp-aaa.md) | S3-S4 | 5.0 |

## Domaines 1.0 et 6.0 (SD-Access, SD-WAN, automation)
Pas de lab local possible → **sandboxes Cisco DevNet** (gratuites) :
- *Catalyst Center Always-On* : appels Intent API avec Postman/Python (token → inventaire → santé)
- *Cisco SD-WAN Always-On* : API vManage (/dataservice)
- *IOS XE on CML Always-On* : NETCONF/RESTCONF réels (ncclient, requests)

Objectif minimal : **une session API complète par sandbox** — le flux token → requête → JSON parsé se retient 10× mieux en le faisant.
