# 🎯 CCNP ENCOR 350-401 — Parcours de préparation

> Dépôt d'étude personnel · examen **Cisco 350-401 ENCOR v1.2** · parcours autonome de 5 semaines, **date d'examen configurable dans le hub** 🇭🇹

Un kit de préparation complet et autonome, déployable en un clic sur **GitHub Pages** : plan d'étude interactif, simulateur de 350 questions, fiches de révision PDF et 10 labs guidés.

## 📁 Structure

```
encor-350-401/
├── index.html              ← Hub d'accueil : liens + suivi de progression 5 semaines
├── plan/index.html         ← Plan d'étude complet (26 modules, calendrier, pièges d'examen)
├── simulateur/index.html   ← Simulateur : 350 questions, blancs de 100 q / 120 min
├── flashcards/index.html   ← 127 flashcards à répétition espacée (Leitner 3 boîtes)
├── strategie/index.html    ← Stratégie de réussite spécial 2ᵉ passage
├── fiches/…pdf             ← 8 pages de fiches imprimables (ports, LSA, FHRP, SDN, jour J)
└── labs/                   ← 10 labs CML-Free/Packet Tracer (.md + version web)
```

## 🚀 Déploiement sur GitHub Pages

1. Crée un dépôt (ex. `encor-350-401`) sur ton compte GitHub
2. Pousse le contenu :
   ```bash
   git init
   git add .
   git commit -m "🎯 Kit de préparation ENCOR 350-401"
   git branch -M main
   git remote add origin https://github.com/<ton-user>/encor-350-401.git
   git push -u origin main
   ```
3. **Settings → Pages → Source : Deploy from a branch → main / (root) → Save**
4. Le site est en ligne sur `https://<ton-user>.github.io/encor-350-401/` ✅

> 💡 La progression des 5 semaines est sauvegardée dans le navigateur (localStorage) : elle persiste de session en session sur le même appareil.

## 🧭 Le parcours en 5 semaines

| Semaine | Focus | Labs |
|---|---|---|
| S1 | Architecture & commutation (STP, EtherChannel, FHRP, QoS) | 01–03 |
| S2 | Routage (OSPF v2/v3, BGP, EIGRP, multicast) | 04–06 |
| S3 | Sécurité & virtualisation (AAA, CoPP, 802.1X, VRF, GRE/IPsec, DMVPN) | 07–10 |
| S4 | SD-Access, SD-WAN, automation (Python, REST, NETCONF/YANG) + blanc n°1 | DevNet |
| S5 | Sprint final : rattrapage ciblé + blancs n°2 et n°3 (objectif ≥ 80 %) | — |

## 🧭 Spécial deuxième tentative

Ce kit est construit pour un **second passage** :
1. **Diagnostic d'abord** : la page Stratégie aide à convertir le score report du 1er passage en plan de priorités (poids blueprint × faiblesse)
2. **Mémorisation active** : flashcards quotidiennes + labs + questions, pas de relecture passive
3. **Mesure continue** : chaque session du simulateur est historisée ; le hub affiche la tendance des examens blancs et les domaines < 70 %
4. **Feu vert objectif** : ≥ 80 % au 3ᵉ blanc en conditions réelles avant de se présenter

## 🎯 Le simulateur en bref

- **350 questions originales** en anglais (la langue de l'examen), explications détaillées en français
- Pondération conforme au blueprint : Infrastructure 30 % · Sécurité 20 % · Architecture 15 % · Automation 15 % · Virtualisation 10 % · Assurance 10 %
- **Mode examen blanc** : tirage aléatoire de 100 questions pondérées, chrono 120 min, correction à la fin
- **Mode étude** (correction immédiate) et **mode par domaine**
- Options mélangées à chaque affichage (anti-mémorisation des positions), résultats par domaine, revue des erreurs

## ⚠️ Honnêteté intellectuelle

Ces questions sont **originales** — ce ne sont pas des dumps (interdits par le NDA Cisco et contre-productifs). Elles visent le style et la difficulté de l'examen mais le calibrage exact reste approximatif : croise avec les quiz **NetAcad** de chaque module et idéalement un **Boson ExSim-Max** avant le jour J.

## ♾️ Conçu pour durer

Aucune maintenance requise : tout est statique et fonctionne hors-ligne une fois chargé. La date d'examen se règle dans le hub (et se modifie librement si tu reportes), le calendrier est exprimé en semaines relatives, et progression / historique / flashcards vivent dans le navigateur. Seule vérification recommandée avant de commencer : les [exam topics officiels](https://learningnetwork.cisco.com/s/encor-exam-topics) sur le site Cisco (ce kit couvre la v1.2, en vigueur depuis septembre 2023 — la version que tu as passée en 2024).

## 📚 Ressources officielles

- [Cisco 350-401 ENCOR — Exam topics](https://learningnetwork.cisco.com/s/encor-exam-topics)
- Cours NetAcad *CCNP ENCOR v9*
- *CCNP and CCIE ENCOR 350-401 Official Cert Guide* (Cisco Press)
- [Cisco DevNet Sandboxes](https://developer.cisco.com/site/sandbox/) — Catalyst Center & SD-WAN Always-On
- [CML-Free](https://developer.cisco.com/modeling-labs/) — labs IOS-XE (5 nœuds gratuits)

---
*Généré avec l'aide de Claude (Anthropic) · contenu pédagogique original · bon courage, kenbe fèm ! 💪*
