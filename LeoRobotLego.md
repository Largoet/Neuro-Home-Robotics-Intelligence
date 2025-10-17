# 🤖 LEO – From LEGO Boost to Real Home Robotics

> *Apprentissage, expérimentation et conception d’un robot domestique intelligent.*

---

## 🧩 **Présentation**

**Leo** est à l’origine un robot LEGO Boost conçu pour le jeu et l’initiation à la robotique.  
Mais l’application officielle **LEGO Boost** est désormais **obsolète** : elle ne fonctionne plus sur les versions récentes d’Android et n’est plus maintenue.  

Ce projet vise donc à **redonner vie à Leo** — en le transformant progressivement en un **robot domestique programmable**, évolutif, et exploitable par mon fils pour apprendre à coder et comprendre la logique robotique.

---

## ⚠️ **Problématique**

L’application LEGO Boost :
- ❌ n’est plus compatible avec les versions récentes d’Android ;
- ❌ limite fortement les interactions et la créativité ;
- ❌ empêche toute communication externe ou connexion réseau.

Résultat : un robot inutilisable à long terme, alors que son potentiel reste immense pour l’apprentissage et la domotique.

---

## 🛠️ **Première parade**

Une **version APK modifiée** de l’application LEGO Boost, maintenue par la communauté, permet encore d’utiliser le robot.  
Elle constitue une **solution temporaire** pour :
- tester les moteurs et capteurs ;
- réaliser les missions d’origine ;
- initier mon fils à la logique de programmation visuelle.

---

## 👨‍🚀 **Apprentissage et transmission**

L’objectif principal est de faire de **Leo un projet éducatif et expérimental**.  
Avec mon fils, nous apprenons :
- comment le robot communique via **Bluetooth Low Energy (BLE)** ;
- comment envoyer des ordres simples en **Python** (`pylgbst`) ;
- comment créer des **séquences d’actions** : avancer, tourner, changer de couleur, détecter une inclinaison, etc.

Cette approche rend la programmation **tangible et concrète**, développe la logique et ouvre la porte à des concepts comme la communication homme-machine.

---

## 🧱 **Limites de la version LEGO**

Malgré son charme, le robot LEGO reste limité :
- alimentation par **6 piles AAA**, offrant peu d’autonomie pour des projets prolongés ;
- dépendance à l’application LEGO fermée ;
- aucune voix ni retour vocal ;
- absence de connectivité Wi-Fi ou de lien direct avec la domotique.

Ces limites empêchent d’en faire un **outil utile à la maison** ou un **projet évolutif** au-delà du jeu.

---

## 🚀 **Vers un Leo 2.0 – Le robot utile et ouvert**

L’objectif à moyen terme (2026-2027) est de créer une **nouvelle version de Leo**,  
basée sur une carte électronique plus ouverte et connectée, telle qu’un **Raspberry Pi Zero 2 W** ou un **ESP32**, équipée de :

| Composant | Fonction |
|------------|-----------|
| 🧠 **Microprocesseur** | gestion du robot, scripts Python, interactions |
| 🎙️ **Micro** | reconnaissance vocale locale |
| 🔊 **Haut-parleur** | réponses audio ou vocales |
| 📶 **Wi-Fi / BLE** | communication avec le réseau domestique |
| 🖥️ **Écran** | affichage d’informations ou d’émotions |
| ⚙️ **Moteurs et capteurs** | contrôle des déplacements et réactions |

Ce **Leo 2.0** pourrait interagir avec **Atlas AI** (Pc herbegeant l'IA avec des composants puissants) et **Kinto-Cloud** (serveur), tout en restant compréhensible et manipulable par un enfant.

---

## 💡 **Idées de scripts à expérimenter**

| Objectif | Description | Technologies |
|-----------|--------------|---------------|
| 🏡 **Communication avec Atlas AI** | envoyer/recevoir des ordres vocaux (“avance de 1 mètre”) | MQTT + Python |
| 🧠 **Dialogue simple** | transformer des commandes vocales en actions concrètes | Whisper + Python |
| ⚙️ **Pilotage moteur** | utiliser `pylgbst` pour piloter les moteurs BLE | Python |
| 🕹️ **Contrôle manuel** | ajouter un mode manette (ZQSD ou Bluetooth) | WebSocket / BLE |
| 📡 **Capteurs maison** | connecter capteurs domotiques (température, lumière) | MQTT / Home Assistant |
| 💬 **Expressions** | animer Leo (yeux, sons, LEDs, mini-écran) | Python / GPIO |

---

## 🧠 **Connexion avec l’écosystème Neuro-Home**

**Leo** s’intègre dans le projet global **Neuro-Home Robotics Intelligence**,  
un environnement local où humains, robots et objets connectés partagent le même réseau :

| Élément | Rôle |
|----------|------|
| **Atlas AI** | cerveau central (IA) |
| **Kinto-Cloud** | réseau domestique avec gestion de la maison et des automatisations (serveur local, home assistant) |
| **Family Robotics** | ensemble des robots connectés (Leo, Neo, Reachy, etc.) |

---

## 🕰️ **Phases de développement**

| Phase | Période estimée | Objectif principal | Description |
|--------|------------------|--------------------|--------------|
| **Phase 1 – Éveil & apprentissage** | 2025 → 2026 | Découverte de la robotique et de la programmation | Utilisation du LEGO Boost et des scripts Python pour comprendre la communication BLE, les moteurs et les actions séquentielles. |
| **Phase 2 – Renaissance de Leo** | 2027 → 2028 | Transformer Leo en robot domestique utile | Nouvelle carte électronique (Wi-Fi, micro, HP, écran), intégration à Kinto-Cloud et au réseau Neuro-Home. |
| **Phase 3 – Extension familiale** | 2028 → ... | Interconnexion avec les autres robots | Communication Leo ↔ Neo ↔ futurs compagnons via le réseau domestique et Home Assistant. |

---

