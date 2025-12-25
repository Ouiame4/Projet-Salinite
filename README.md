# 🌱 Système IoT de Surveillance de Salinité & Aide à la Décision (Maroc)

Ce projet est une solution IoT complète ("End-to-End") pour surveiller la salinité des sols agricoles dans les zones côtières marocaines (ex: Saïdia). Il combine l'acquisition de données physiques, le traitement local (Edge Computing) et une intelligence agronomique pour fournir des recommandations actionnables via Telegram.

## Fonctionnalités Clés

- **Surveillance Temps Réel :** Mesure continue de la Conductivité Électrique (EC) et conversion en TDS (ppm).

- **Edge Computing (ESP32) :**

  - Filtrage Numérique : Filtre médian et lissage exponentiel pour éliminer le bruit des capteurs low-cost.
  
  - Machine à États : Gestion intelligente des notifications pour éviter le "spam" d'alertes.
  
  - Intelligence Contextuelle : Adaptation des seuils et conseils selon la région géographique configurée.
  
  - Système Expert Embarqué : Génération de conseils précis (Irrigation, Lessivage, Amendement) sans dépendre du Cloud.

- **Dashboard Cloud :** Visualisation historique et temps réel sur ThingsBoard.

- **Alertes Mobiles :** Notifications riches via Telegram avec émojis et plans d'action.

## Architecture Matérielle

- **Microcontrôleur :** ESP32 DevKit V1 (Wi-Fi intégré).

- **Capteur :** Sonde TDS analogique (Total Dissolved Solids).

- **Alimentation :** 5V / 3.3V via Micro-USB.

## Installation & Configuration

### 1. Prérequis

Arduino IDE avec le support ESP32 installé.

Bibliothèques nécessaires (à installer via le Gestionnaire de bibliothèques) :
```bash
PubSubClient (Client MQTT)

WiFi (Standard ESP32)

HTTPClient & WiFiClientSecure (Standard ESP32)

Preferences (Standard ESP32)
```
### 2. Configuration du Firmware

Ouvrez le fichier source et modifiez la section CONFIGURATION UTILISATEUR avec vos propres identifiants :

```bash
// --- 1. WiFi & Cloud ---
const char* ssid        = "VOTRE_WIFI";
const char* password    = "VOTRE_MOT_DE_PASSE";
const char* token       = "VOTRE_TOKEN_THINGSBOARD"; 

// --- 2. Telegram ---
const char* bot_token   = "VOTRE_BOT_TOKEN";
const char* chat_id     = "VOTRE_CHAT_ID";

// --- 3. Géographie ---
String REGION_CIBLE = "SAIDIA"; // Choix : SAIDIA, AGADIR, DAKHLA...
```
###  Comment Obtenir Vos Identifiants (Tokens)

Pour que le système fonctionne, vous devez créer vos propres clés d'accès. Voici la procédure :

### 1. Token ThingsBoard (MQTT)
1.  Créez un compte gratuit sur [ThingsBoard Demo](https://demo.thingsboard.io/).
2.  Allez dans l'onglet **"Devices"** (Appareils).
3.  Cliquez sur le bouton **"+"** pour ajouter un nouvel appareil.
4.  Nommez-le (ex: `ESP32_Salinite`).
5.  Une fois créé, cliquez sur l'appareil dans la liste pour ouvrir ses détails.
6.  Cliquez sur le bouton **"Copy Access Token"**.
7.  Collez ce token dans la variable `token` du code.

### 2. Token Bot Telegram
1.  Ouvrez l'application Telegram et cherchez l'utilisateur **@BotFather**.
2.  Envoyez la commande `/newbot`.
3.  Donnez un nom à votre bot (ex: `Mon_Projet_IoT_Bot`).
4.  Donnez un nom d'utilisateur (doit finir par `bot`, ex: `SaliniteMarocBot`).
5.  BotFather vous donnera un **Token d'accès** (ex: `123456:ABC-DEF...`).
6.  Collez ce token dans la variable `bot_token` du code.

### 3. Chat ID Telegram
1.  Lancez une conversation avec votre nouveau bot (cliquez sur "Start").
2.  Cherchez un autre bot nommé **@userinfobot** ou **@IDBot**.
3.  Envoyez n'importe quel message à ce bot.
4.  Il vous répondra avec votre **"Id"** (un nombre, ex: `123456789`).
5.  Collez ce nombre dans la variable `chat_id` du code.

### 3. Branchement

| Composant | ESP32 Pin |
| :--- | :--- |
| **Sonde TDS (Signal)** | Broche **34** (Analog Input) |
| **VCC** | 3.3V |
| **GND** | GND |

Format des Données (JSON)

Le système publie les données sur le topic MQTT : v1/devices/me/telemetry

```bash
{
  "tds": 845.2,
  "etat": "ALERTE",
  "region": "Nord / Oriental",
  "tendance": 12.5,
  "conseil": "LESSIVAGE IMMÉDIAT"
}
```


