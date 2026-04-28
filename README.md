# SAÉ 3.02 – Application Android Client/Serveur TCP/IP

> **Boyer Maxence** – BUT Réseaux & Télécommunications, RT2  
> IUT de Roanne – Université Jean Monnet – 2025/2026

---

##  Présentation

Application Android bidirectionnelle permettant de basculer entre **mode client** et **mode serveur** sur un réseau local. Elle supporte les protocoles **TCP et UDP**, gère plusieurs connexions simultanées via du multithreading, et propose un système multi-services avec authentification.

---

##  Fonctionnalités implémentées

- **Dual mode** : une seule application, bascule client ↔ serveur via un `SwitchCompat`
- **TCP & UDP** : deux protocoles supportés avec leurs spécificités (`Socket` / `DatagramSocket`)
- **Données sérialisées** : les échanges utilisent un objet `Message` (`Serializable`) transmis via `ObjectOutputStream`
- **Multithreading** : gestion de plusieurs clients en parallèle via un `ExecutorService` (`newCachedThreadPool`)
- **Multi-services** : le serveur route les requêtes via un `switch` sur la commande reçue
  - `DATE` → retourne la date/heure serveur
  - `ECHO:<texte>` → retourne le texte en miroir
  - `CALCUL:<expression>` → effectue et retourne le résultat d'un calcul
- **Droits d'accès** : authentification login/mot de passe vérifiée via une `HashMap` avant tout traitement
- **Service Android** : le serveur tourne en arrière-plan via `ServerService.java` (hérite de `android.app.Service`) et reste actif même si l'application est fermée
- **Interface Material Design** : `ConstraintLayout`, `MaterialCardView`, `TextInputLayout` avec libellés flottants et affichage/masquage du mot de passe

---

##  Architecture

```
app/
├── manifests/
│   └── AndroidManifest.xml          # Déclaration des activités, du service et des permissions
├── java/
│   └── com.example.sae302/
│       ├── MainActivity.java         # Interface utilisateur (télécommande du service)
│       ├── ServerService.java        # Logique réseau serveur (TCP/UDP, threads, multi-services)
│       └── Message.java              # Objet sérialisable encapsulant username, password, command
└── res/
    ├── layout/activity_main.xml      # Interface Material Design
    └── values/                       # Couleurs, thème, chaînes
```

### Flux de communication

```
[Client]                          [ServerService]
   |                                     |
   |── Message(user, pwd, "DATE") ──────►|
   |                                     |── Authentification
   |                                     |── Switch(command)
   |◄── "2026-04-28 10:52:12" ──────────|
```

---

##  Stack technique

| Composant | Technologie |
|---|---|
| Langage | Java (pas Kotlin) |
| UI | XML + Material Design Components |
| IDE | Android Studio |
| Compilation | Gradle |
| Réseau TCP | `Socket` / `ServerSocket` |
| Réseau UDP | `DatagramSocket` / `DatagramPacket` |
| Sérialisation | `ObjectOutputStream` / `Serializable` |
| Threads | `ExecutorService` (`newCachedThreadPool`) |
| Background | `android.app.Service` + `BroadcastReceiver` |

---

##  Installation

### Prérequis

- Android Studio installé
- SDK Android configuré
- Java 8+

### Étapes

```bash
git clone https://github.com/Maxence0587416/SAE302-Client-Serveur.git
```

1. Ouvrir le projet dans Android Studio
2. Laisser Gradle synchroniser les dépendances
3. Lancer sur un émulateur AVD ou un appareil physique connecté en USB

>  Le protocole UDP ne fonctionne pas de manière fiable sur les émulateurs Android. Utiliser deux appareils physiques connectés au même réseau WiFi pour tester UDP.

---

## 🧪 Utilisation

### Mode Serveur

1. Activer le toggle **Server Mode**
2. Choisir le protocole (TCP ou UDP)
3. Renseigner le port (ex : `8080`)
4. Appuyer sur **Start Server**
5. L'adresse IP s'affiche automatiquement dans le champ dédié

### Mode Client

1. Laisser le toggle sur Client Mode
2. Choisir le protocole
3. Renseigner l'IP et le port du serveur
4. Saisir le nom d'utilisateur et le mot de passe
5. Entrer une commande (`DATE`, `ECHO:texte`, `CALCUL:30+12`)
6. Appuyer sur **Send Message**


---

## 📄 Licence

Projet académique – IUT de Roanne, BUT RT 2025/2026. Non destiné à un usage commercial.
