
# 🏠 Neuro-Home-Robotics-Intelligence
**Un assistant domestique local et intelligent, mêlant IA, robotique et domotique.**

---

## 💡 Vision du projet
Créer un **assistant personnel familial** capable d’interagir naturellement avec la maison et ses habitants — sans cloud, sans espionnage, et 100 % local.  

Reachy Home Intelligence, c’est une symbiose entre :  
- 🤖 **Reachy Mini** : le visage, la voix et la présence physique de l’assistant ;  
- 🧠 **Atlas** : le cerveau IA local, propulsé par un GPU AMD et les modèles open source (Mistral, Whisper, Piper) ;  
- 🏡 **Kinto-Cloud** : le cœur domotique permanent, piloté par Home Assistant ;  
- ⚙️ **Réseau Zigbee local** : ampoules, prises, capteurs et automatisations.  

Le tout relié dans un écosystème **sécurisé, local et open-source**.

## ⚙️ Architecture générale


        🎙️ Reachy Mini
     (Raspberry Pi / interface vocale)
               │
        [openWakeWord + Piper + MQTT]
               │
               ▼
      🏡 Kinto-Cloud (serveur permanent)
    ├── Home Assistant
    ├── Zigbee2MQTT
    ├── Wake-on-LAN (réveille Atlas)
    ├── Capteurs & automatisations
    └── Intent scripts (volets, lumières, etc.)
               │
               ▼
         🧠 Atlas (gros PC GPU)
    ├── Ollama (Mistral / Mixtral)
    ├── Faster-Whisper (STT)
    ├── Piper (TTS)
    └── Orchestrateur IA (logique / dialogue)



---

## 🧩 Rôles des machines

### 🧠 **Atlas** — *le cerveau IA*
- GPU AMD Radeon RX 9070 – 16 Go VRAM  
- 32 Go RAM, Ryzen 7 5700X  
- Héberge :  
  - `Ollama` → modèles IA (Mistral 7B / Mixtral 8x7B)  
  - `Faster-Whisper` → transcription vocale  
  - `Piper` → synthèse vocale  
  - `Orchestrator` → logique de dialogue et coordination  

💡 Atlas fonctionne uniquement quand la maison est active (réveil via **Wake-on-LAN** à 8 h, arrêt nocturne).  

> 🧱 Isolation : les services IA tournent dans des **conteneurs Docker** avec accès GPU.  
> - Pas de `--privileged`  
> - Utilisateur non-root  
> - Système de fichiers en lecture seule  
> - Réseau Docker privé (ports exposés uniquement en `127.0.0.1`)  

---

### 🏡 **Kinto-Cloud** — *le cœur domotique H24*
- Intel i5-6500 / 8 Go RAM / RAID 1  
- Héberge :  
  - `Home Assistant` (automatisations, météo, routines)  
  - `Zigbee2MQTT` (ampoules, prises, capteurs)  
  - `Wake-on-LAN` (réveil automatique d’Atlas)  
  - `MQTT broker`  
  - `Piper` (voix locale simple)  

🌙 Reste allumé jour et nuit pour :  
- gérer la lumière selon lever/coucher du soleil,  
- mesurer la consommation électrique,  
- annoncer la météo du matin,  
- exécuter les commandes simples sans IA.

---

### 🤖 **Reachy Mini** — *le visage et la voix*
- Raspberry Pi intégré (version Wireless)  
- Gère :  
  - détection vocale (`openWakeWord`)  
  - mouvements et expressions  
  - synthèse vocale (`Piper` local)  
  - communication MQTT/HTTP vers Kinto-Cloud et Atlas  

💬 Reachy Mini incarne l’assistant de la maison.  
Il fonctionne même sans Atlas pour les ordres simples (“éteins la lumière”, “mets une alarme”).

![REACHY-MINI-34-scaled](https://github.com/user-attachments/assets/c70dd1d2-60eb-4bf1-8a06-7f444604254e)


---

## 🧠 Scénarios typiques

| Contexte | Action | Comportement |
|-----------|---------|--------------|
| 🌅 Matin | “Reachy, bonjour.” | Lumière chaude 20 %, réveil d’Atlas, annonce météo. |
| 🧒 Devoirs | “Reachy, on fait les devoirs ?” | Atlas active Mistral, aide pédagogique adaptée. |
| 🌙 Nuit | Passage couloir | Lumière 5 % / 1 min, silence total. |
| 🖨️ Bureau | “Reachy, allume l’imprimante.” | Home Assistant active la prise Zigbee. |
| 🎮 Jeu | Lancement de jeu | Atlas stoppe les conteneurs IA pour libérer le GPU. |
---
## ⚡ Matériel recommandé (v1)

| Élément | Rôle | Modèle conseillé |
|----------|------|-----------------|
| 🧠 Cerveau IA | Atlas | Ryzen 7 + Radeon RX 9070 |
| 🏡 Cœur domotique | Kinto-Cloud | i5-6500 + Home Assistant |
| 🤖 Interface | Reachy Mini Wireless | Raspberry Pi intégré |
| 📡 Réseau Zigbee | Communication | Sonoff ZBDongle-E |
| 💡 Ampoule | Lumière chaude/froide | IKEA TRÅDFRI CCT |
| 🔌 Prise | Conso + contrôle | Aqara T1 / Lidl Zigbee |
| 👀 Capteur | Mouvement | Aqara Motion P1 |

---

## 🚀 Objectifs v1
- [ ] Installer Home Assistant sur Kinto-Cloud  
- [ ] Connecter ampoule et prise Zigbee  
- [ ] Créer automatisations (lumière, météo, réveil PC)  
- [ ] Intégrer Wake-on-LAN + MQTT  
- [ ] Déployer Ollama, Whisper, Piper sur Atlas  
- [ ] Faire parler Reachy Mini avec les services locaux  

---
---

## 🧩 Mise en place technique : installation et orchestration

### 🧠 1. Atlas — Installation de l’IA locale

**Objectif :** héberger les modèles d’intelligence artificielle en local (Mistral, Whisper, Piper) sans cloud.

#### Étapes principales :
1. **Installer Docker et Docker Compose**  
   - Pour isoler chaque service IA et simplifier les mises à jour.  
   - Compose orchestre Ollama (Mistral), Whisper et Piper dans un environnement cohérent.

2. **Créer une architecture modulaire**  
   - `Ollama` (Mistral) → génération de texte, raisonnement, conversation.  
   - `Whisper` → transcription vocale.  
   - `Piper` → synthèse vocale.  
   - Chaque service communique via API interne et ports sécurisés.

3. **Configurer des volumes persistants**
   - Sauvegarde des modèles téléchargés, logs et fichiers de config (`/data/ai`).
   - Les conteneurs peuvent être redéployés sans perte de données.

4. **Accès GPU (Radeon RX 9070)**
   - Activation ROCm pour exploitation du GPU dans les conteneurs.  
   - Accélération x5 à x10 des traitements IA → latence moyenne < 3 s.

5. **Exposition d’une API locale (HTTP)**
   - Ollama expose une API REST sur `http://localhost:11434` accessible par Home Assistant ou Reachy Mini.

6. **Service de démarrage automatique**
   - Un service `ollama.service` ou équivalent démarre automatiquement les conteneurs IA à l’allumage d’Atlas.

---

### 🏡 2. Kinto-Cloud — Installation du cœur domotique

**Objectif :** centraliser la gestion des appareils et automatisations 24/7 via Home Assistant.

#### Étapes principales :
1. **Installer Home Assistant (en conteneur ou dédié)**  
   - Environnement stable, redéployable, compatible Docker.

2. **Connecter le réseau Zigbee**
   - Dongle Zigbee (Sonoff ZBDongle-E) branché sur Kinto-Cloud.  
   - Communication radio avec ampoules, prises et capteurs.

3. **Installer les intégrations nécessaires**
   - `Zigbee2MQTT` pour traduire les signaux,  
   - `Mosquitto` (broker MQTT),  
   - `Home Assistant Energy` pour la mesure de consommation.

4. **Créer des automatisations de base**
   - Coupure automatique des écrans la nuit,  
   - Fin de cycle LL/LV détectée et coupure d’alim,  
   - Scènes lumineuses (matin doux, soirée tamisée, nuit à 5 %).

5. **Sauvegardes et logs**
   - Volume `/homeassistant/config` sauvegardé sur le RAID `/mnt/raid`.

---

### 🗣️ 3. Communication IA ↔ Domotique ↔ Interface Reachy

**Flux logique :**
Voix → Micro (Reachy Mini / PC) → Whisper → Mistral (via Ollama)
→ Réponse texte → Piper (voix synthétique)
→ Commande Home Assistant via API ou MQTT → Action domotique

#### Étapes :
1. **Entrée vocale :**
   - Le micro capture la voix → Whisper transcrit le texte.  
   - Whisper envoie la commande à Mistral via Ollama.

2. **Traitement IA :**
   - Mistral répond en texte.  
   - Si réponse = commande domotique, transmission vers Home Assistant.  
   - Sinon, Piper génère une voix naturelle pour réponse orale.

3. **Sortie et actions :**
   - Home Assistant exécute la commande reçue (`light.turn_on`, `media.play`, etc.).
   - Whisper/Piper restent en écoute pour les requêtes suivantes.

4. **Sécurité et réseau :**
   - Toutes les communications restent internes (pas d’exposition Internet).  
   - HTTPS + SSH + VLAN IoT pour cloisonner les flux.

---

### ⚙️ 4. Performances, énergie et supervision intelligente

**Objectif :** garantir une expérience fluide, sobre et continue.

#### 💻 Gestion intelligente d’Atlas

Atlas est puissante, mais inutile la nuit.  
Elle s’éteint proprement après une période d’inactivité **après minuit**, sauf si l’utilisateur repousse la coupure.

##### 🔸 Comportement :
- Surveillance d’activité (souris, clavier, charge CPU/GPU, ou requêtes IA).  
- Si **aucune activité détectée après minuit (00h15)** :
  - Envoi d’un **popup** :  
    “💤 Coupure planifiée d’Atlas dans 5 minutes — reporter ou annuler ?”  
  - Si pas de réponse → **shutdown propre** :
    - arrêt des conteneurs IA,  
    - sauvegarde des sessions,  
    - extinction coordonnée avec la **multiprise connectée** (écrans inclus).
- **Wake-on-LAN automatique** à 07h00 ou à la demande via Home Assistant.

> 🔍 Le système s’assure que l’arrêt se fasse toujours dans le bon ordre (IA → écrans → prise).  
> Ainsi, aucune coupure brutale ni corruption de données.

---

#### 🔌 Relais IA allégé (Whisper + Piper)

Quand Atlas est éteint, un **relais minimal** prend le relais sur **Kinto-Cloud** :
- **Whisper** reste actif pour écouter les commandes locales.  
- **Piper** continue de répondre vocalement aux requêtes simples :
  - météo, rappel, heure, allumage de lampe, etc.  
- Les requêtes complexes sont différées jusqu’au redémarrage d’Atlas.

> 🧠 Cela garantit une **présence vocale permanente** (assistant “réduit”),  
> tout en gardant **Atlas endormi** la nuit pour économiser énergie et bruit.

---

#### 📊 Monitoring et alertes

- **Prometheus + Grafana** : suivi de la charge CPU/GPU, des prises connectées, du réseau Zigbee.  
- **Alertes Home Assistant** :
  - charge GPU anormale,  
  - absence de redémarrage d’Atlas,  
  - shutdown repoussé plusieurs fois (activité nocturne répétée).
---
## ⚡ Analyse énergétique et rentabilité de la domotique

Cette section compare la consommation et le coût énergétique **avant et après** l’intégration de la domotique Home Assistant (prises connectées, ampoules intelligentes, automatisations).  
L’objectif est de mesurer **l’impact réel** du système sur la consommation électrique et le **retour sur investissement (ROI)** à moyen et long terme.

---

### 🔧 Hypothèses de départ

- Tarif électricité : **0,20 €/kWh** (et lecture à 0,25 €/kWh pour projection).
- Ampoules LED classiques : **9 W**, non dimmables.
- Ampoules connectées Zigbee CCT : **12 € / pièce**, 20 000 h de durée de vie (~15–25 ans selon usage).
- Prises connectées mesurées : **15 € / unité**, durée de vie ~10 ans.
- Dongle Zigbee : **25 €** (achat unique).
- Capteur de mouvement : **20 €**.
- Économies mesurées : **écrans coupés la nuit, veilles supprimées, lumières oubliées, dimming matin/soir**.

---

### ⚖️ Consommation annuelle – Classique vs Domotique

| Poste | Classique (sans domotique) | Domotique (HA + prises/ampoules) | Économie |
|---|---:|---:|---:|
| **Écrans oubliés la nuit** (2 chez toi + 1 Mme) | ~132 kWh | ~10 kWh | **~122 kWh** |
| **Imprimante** (oublis + veille) | ~30 kWh | ~8 kWh | **~22 kWh** |
| **LL/LV** (fin de cycle + veilles) | ~25 kWh | ~10 kWh | **~15 kWh** |
| **Lumières oubliées** (SDB/chambres/salon) | ~25 kWh | ~10 kWh | **~15 kWh** |
| **Dimming matin/soir** (3 pièces) | 0 kWh | –15 kWh (éco) | **~15 kWh** |
| **Total pilotable** | **~212 kWh/an** | **~53 kWh/an** | **🟩 ~160 kWh/an** |

> 💰 À 0,20 €/kWh → **~32 € d’économies / an**  
> 💰 À 0,25 €/kWh → **~40 € d’économies / an**

---

### 💼 Coût du matériel et amortissement

| Panier | Détail | Coût initial |
|---|---|---:|
| **Minimal** | 4 prises + 2 ampoules CCT + 1 dongle Zigbee + 1 capteur mouvement | **129 €** |
| **Étendu** | 6 prises + 6 ampoules CCT + 1 dongle + 1 capteur | **207 €** |

Durée de vie estimée :
- **Prises** : 10 ans (remplacement partiel à mi-parcours).
- **Ampoules** : 20 000 h (soit 15–25 ans).
- **Dongle / capteur** : 10 ans minimum.

---

### 📈 Retour sur investissement (ROI)

#### À 0,20 €/kWh

| Horizon | Minimal (129 €) | Étendu (207 €) |
|---|---:|---:|
| **1 an** | –97 € | –167 € |
| **5 ans** | +21 € | –7 € |
| **10 ans** | +111 € | +103 € |
| **20 ans** | +405 € | +467 € |

#### À 0,25 €/kWh

| Horizon | Minimal | Étendu |
|---|---:|---:|
| **1 an** | –91 € | –157 € |
| **5 ans** | +58 € | +43 € |
| **10 ans** | +186 € | +203 € |
| **20 ans** | +555 € | +667 € |

> 🔁 **ROI** : entre **3 et 5 ans** selon configuration et prix du kWh.  
> Au-delà de 10 ans, la domotique devient **très rentable**, avec un **gain net de 100–200 €** (0,20 €/kWh) et jusqu’à **300–600 €** (0,25 €/kWh).

---

### 💡 Détails par poste

| Poste | Action domotique | Économie estimée (kWh/an) | Économie (€) |
|---|---|---:|---:|
| **Écrans** | Coupure automatique 01:00→07:00 | ~130 | 26 € |
| **Imprimante** | Auto-off après inactivité / coupure nuit | ~25 | 5 € |
| **LL/LV** | Coupure veille après fin de cycle | ~15 | 3 € |
| **Lumières** | Capteurs + minuterie | ~15 | 3 € |
| **Dimming** | Réduction soir/matin | ~15 | 3 € |
| **Total annuel** | — | **~200 kWh/an** | **~40 €** |

---

### 🧱 Durabilité & remplacements

| Composant | Durée de vie moyenne | Coût unitaire | Remplacement prévu |
|---|---|---:|---:|
| **Ampoule connectée Zigbee (CCT)** | 20 000 h (~15–25 ans) | 12 € | 50 % du set à 15–20 ans |
| **Prise connectée Zigbee** | ~10 ans | 15 € | remplacement complet à 10 ans |
| **Dongle Zigbee** | ~10 ans | 25 € | non critique |
| **Capteur mouvement** | ~10 ans | 20 € | non critique |

> 💬 L’ampoule se remplace entièrement, mais **on ne jette pas de passerelle ni de hub**.  
> Ces composants se changent aussi rarement que des lampes classiques.

---

### 🧠 Conclusion

- Le **système domotique Home Assistant** permet de réduire la consommation du foyer de **~160–200 kWh/an** (~40 €/an) sans effort humain.  
- Le **ROI moyen** est atteint **entre 3 et 5 ans**, ensuite **le système devient générateur d’économies nettes**.  
- L’investissement est **pérenne**, car les ampoules et prises durent entre **10 et 25 ans**.  
- Le confort s’améliore :  
  - Lumières douces au réveil et la nuit,  
  - Aucune lampe oubliée,  
  - Fin automatique des veilles et cycles.  

En bref, la domotique **ne révolutionne pas la facture électrique**, mais elle **stabilise la consommation, supprime les gaspillages invisibles et améliore la qualité de vie** — tout en servant de **laboratoire DevOps / MLOps concret** pour l’apprentissage.


---

## 🧱 Isolation et performances IA

| Méthode | Usage | Avantage | Inconvénient |
|----------|-------|-----------|---------------|
| 🐳 **Containers Docker/Podman** | par défaut | Accès GPU natif, isolation légère, maintenance simple | isolation moyenne |
| 🧩 **VM (KVM/QEMU)** | optionnel | cloisonnement total, snapshots | complexité, perte perfs GPU |

---

## 🔒 Sécurité & Résilience

L’écosystème *Reachy Home Intelligence* repose sur une **philosophie “Local First + Zero Trust”**.

### 🧱 1. Cloisonnement réseau  
- Réseau Wi-Fi/VLAN IoT séparé (`IoT_Network`)  
- Communication autorisée uniquement entre :  
  - Reachy ↔ Kinto-Cloud  
  - Atlas ↔ Kinto-Cloud  
- Blocage du trafic sortant inutile (pas de cloud Tuya/Amazon).  

### 🔑 2. Authentification & chiffrement  
- HTTPS obligatoire sur Home Assistant.  
- Clés SSH et tokens restreints pour API internes.  
- VPN (WireGuard/Tailscale) pour tout accès distant.  

### 🧠 3. Sécurité applicative  
- Services en conteneurs isolés.  
- Pas de `--privileged`, utilisateur non-root.  
- Images Docker vérifiées et signées.  

### 🔍 4. Surveillance  
- Logs centralisés sur Kinto-Cloud (`/mnt/raid/logs`).  
- Supervision légère : Prometheus + Grafana / Loki.  
- Alertes locales sur redémarrages ou connexions suspectes.  

### ⚙️ 5. Sauvegardes & continuité  
- Sauvegardes chiffrées automatiques (borgbackup / rsync).  
- Export mensuel externe (SSD/clé USB).  
- `restart: unless-stopped` sur tous les services critiques.  


---



## 🌟 Vision long terme
- 🤝 Mémoire contextuelle locale  
- 🎓 Aide aux devoirs enrichie  
- 🧩 Profils personnalisés (parent/enfant/travail)  
- 🔐 Tableau de bord web local  
- ⚡ Optimisation énergétique prédictive  

---

## ❤️ Philosophie du projet
> “Un assistant qui comprend la maison et ses habitants 
> pas de service cloud qui les observe.”

Reachy Home Intelligence incarne un **foyer intelligent autonome et éthique**.

