# Projet d'Arrosage Automatique avec ESP32

## Description
Ce projet utilise un microcontrôleur ESP32 pour surveiller l'humidité du sol à l'aide de quatre capteurs d'humidité et contrôler deux relais pour l'arrosage automatique ou manuel. Les données des capteurs sont envoyées à un serveur via une API REST, et les commandes (mode automatique ou manuel, état des vannes) sont récupérées depuis le serveur. Une LED indique l'état du système.

## Fonctionnalités
- **Mesure de l'humidité** : Lit les valeurs de quatre capteurs d'humidité et calcule une moyenne.
- **Contrôle des relais** : Active ou désactive deux vannes (relais) en fonction du mode (automatique ou manuel).
  - **Mode automatique** : Active l'arrosage si l'humidité moyenne est inférieure à un seuil (35 %).
  - **Mode manuel** : Contrôle les vannes selon les commandes reçues du serveur.
- **Connexion Wi-Fi** : Se connecte à un réseau Wi-Fi pour communiquer avec un serveur distant.
- **Communication API** :
  - Envoie les données des capteurs (humidité, température, état des vannes, mode) à une URL définie.
  - Récupère les commandes (mode, état des vannes) depuis le serveur.
- **Gestion des erreurs** : Vérifie la connexion Wi-Fi, gère les échecs de communication avec des tentatives multiples, et signale les erreurs via une LED.

## Matériel requis
- **ESP32** : Microcontrôleur avec capacité Wi-Fi.
- **Capteurs d'humidité** : 4 capteurs analogiques connectés aux pins 32, 33, 35, et 34.
- **Relais** : 2 relais connectés aux pins 19 (tap1) et 5 (tap2) pour contrôler les vannes.
- **LED** : Connectée à la pin 14 pour indiquer l'état du système.
- **Routeur Wi-Fi** : Pour la connexion au réseau.
- **Serveur** : Serveur avec API REST fonctionnant sur les endpoints `/api/sensor-data/` (POST) et `/api/control/` (GET).

## Configuration
### Connexion Wi-Fi
- **SSID** : Nom du réseau Wi-Fi (par ex., `CAEB_NATI`).
- **Mot de passe** : Clé du réseau Wi-Fi (par ex., `@caebnati@`).
- **URLs du serveur** :
  - Envoi des données : `http://192.168.6.107:8000/api/sensor-data/`
  - Récupération des commandes : `http://192.168.6.107:8000/api/control/`

### Pins utilisées
- **Relais** : Pin 19 (tap1), Pin 5 (tap2).
- **LED** : Pin 14.
- **Capteurs d'humidité** : Pins 32, 33, 35, 34 (ADC avec atténuation 11 dB).

### Paramètres
- **Seuil d'humidité** : 35 % (modifiable via `SEUIL_HUMIDITE`).
- **Répétitions pour la lecture des capteurs** : 10 mesures par capteur pour calculer une moyenne.
- **Délai entre boucles** : 3 secondes.
- **Tentatives de connexion/recommunication** : 3 tentatives maximum avec un délai de 5 secondes entre chaque tentative.

## Installation
1. **Matériel** :
   - Connectez les capteurs d'humidité aux pins 32, 33, 35, et 34.
   - Connectez les relais aux pins 19 et 5.
   - Connectez la LED à la pin 14.
2. **Logiciel** :
   - Installez MicroPython sur l'ESP32.
   - Chargez le script Python fourni sur l'ESP32.
3. **Configuration** :
   - Modifiez les variables `SSID`, `PASSWORD`, `SERVER_URL`, et `CONTROL_URL` selon votre configuration réseau et serveur.
4. **Dépendances** :
   - Bibliothèques MicroPython : `machine`, `network`, `urequests`, `ujson`, `time`, `gc`.

## Utilisation
1. **Démarrage** :
   - L'ESP32 tente de se connecter au Wi-Fi.
   - En cas d'échec après 15 secondes, le programme s'arrête.
2. **Fonctionnement** :
   - Lit les valeurs des capteurs toutes les 3 secondes.
   - En mode automatique, active les relais si l'humidité moyenne est inférieure à 35 %.
   - En mode manuel, suit les commandes reçues du serveur pour les vannes.
   - Envoie les données (humidité des capteurs, moyenne, température fictive, état des vannes, mode) au serveur.
   - Récupère les commandes (mode, état des vannes) depuis le serveur.
3. **Indicateurs** :
   - La LED s'allume lorsque l'arrosage est désactivé (humidité suffisante ou vannes éteintes en mode manuel).
   - La LED clignote 3 fois en cas d'échec d'envoi des données.

## Dépannage
- **Échec de connexion Wi-Fi** :
   - Vérifiez le SSID et le mot de passe.
   - Assurez-vous que l'ESP32 est à portée du routeur.
- **Échec de communication avec le serveur** :
   - Vérifiez que le serveur est accessible à l'adresse IP et au port configurés.
   - Assurez-vous que les endpoints `/api/sensor-data/` et `/api/control/` sont correctement configurés.
- **Valeurs de capteurs invalides** :
   - Vérifiez les connexions des capteurs.
   - Assurez-vous que les capteurs sont alimentés correctement et que l'atténuation ADC est appropriée.

## Améliorations possibles
- Ajouter un capteur de température réel pour remplacer la valeur fictive (25 °C).
- Implémenter un mode de secours local en cas de perte prolongée de connexion Wi-Fi.
- Ajouter une interface locale (par ex., écran OLED) pour afficher l'état en temps réel.
- Permettre la configuration dynamique du seuil d'humidité via le serveur.

## Avertissement
- Assurez-vous que l'alimentation des relais et des vannes est compatible avec l'ESP32.
- Testez le système dans un environnement contrôlé pour éviter les dommages liés à l'eau.

## Auteur
Ce projet a été développé pour un système d'arrosage automatisé avec contrôle à distance via une API REST.

## Licence
Ce projet est sous licence MIT. Vous êtes libre de l'utiliser, le modifier et le distribuer selon les termes de cette licence.
