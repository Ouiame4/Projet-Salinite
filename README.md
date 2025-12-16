🌱 Système IoT de Surveillance de Salinité & Aide à la Décision (Maroc)

Ce projet est un système complet ("End-to-End") de surveillance de la salinité des sols pour l'agriculture côtière marocaine. Il combine l'acquisition de données physiques, le traitement local (Edge Computing) et une intelligence contextuelle pour fournir des recommandations agronomiques actionnables via Telegram.

🚀 Fonctionnalités Clés

Surveillance Temps Réel : Mesure continue de la Conductivité Électrique (EC) et conversion en TDS (ppm).

Edge Computing (ESP32) :

Filtrage Numérique : Filtre médian et lissage exponentiel pour éliminer le bruit.

Anti-Rebond : Validation de la stabilité du signal avant alerte.

Intelligence Contextuelle : Adaptation des seuils et conseils selon la région géographique (ex: Saïdia, Agadir).

Système Expert Embarqué : Moteur de règles générant des conseils précis (Irrigation, Lessivage, Amendement).

Alertes Intelligentes : Notifications riches via Telegram (avec émojis et conseils) uniquement en cas de changement d'état ou de danger persistant.

Dashboard Cloud : Visualisation historique et temps réel sur ThingsBoard.

🛠️ Architecture Matérielle

Microcontrôleur : ESP32 (Wi-Fi intégré).

Capteur : Sonde TDS analogique (Total Dissolved Solids).

Alimentation : 5V / 3.3V.

📦 Installation & Configuration

1. Prérequis

Arduino IDE avec le support ESP32 installé.

Bibliothèques nécessaires :

PubSubClient (pour MQTT)

WiFi (Standard ESP32)

HTTPClient & WiFiClientSecure (Standard ESP32)

Preferences (Standard ESP32)

2. Configuration du Firmware

Ouvrez le fichier .ino et modifiez la section CONFIGURATION UTILISATEUR :

// --- 1. WiFi & Cloud ---
const char* ssid        = "VOTRE_WIFI";
const char* password    = "VOTRE_MOT_DE_PASSE";
const char* token       = "VOTRE_TOKEN_THINGSBOARD"; 

// --- 2. Telegram ---
const char* bot_token   = "VOTRE_BOT_TOKEN";
const char* chat_id     = "VOTRE_CHAT_ID";

// --- 3. Géographie ---
String REGION_CIBLE = "SAIDIA"; // Choix : SAIDIA, AGADIR, DAKHLA...


3. Branchement

Sonde TDS (Signal) -> Broche 34 (Analog Input) de l'ESP32.

VCC -> 3.3V

GND -> GND

📊 Visualisation

Le système publie les données sur le topic MQTT : v1/devices/me/telemetry
Format JSON :

{
  "tds": 845.2,
  "etat": "ALERTE",
  "region": "Nord / Oriental",
  "tendance": 12.5,
  "conseil": "LESSIVAGE IMMÉDIAT"
}


👨‍💻 Auteur

Ouiame Makhoukh Élève Ingénieure en Data Science & Cloud Computing (IDSCC)

École Nationale des Sciences Appliquées d'Oujda (ENSAO).

Projet réalisé dans le cadre du module IoT - Encadré par Prof Kamal AZGHIOU.
