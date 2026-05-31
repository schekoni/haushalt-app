# Firebase-Sync einrichten (kostenlos, ~5 Minuten)

Damit Koni-iPhone, Claude-iPhone und iPad denselben Punktestand teilen.
Firebase ist für so eine kleine App **dauerhaft gratis** (Spark-Plan, keine Kreditkarte nötig).

## 1. Firebase-Projekt erstellen
1. Geh auf https://console.firebase.google.com → mit Google-Konto einloggen
2. **„Projekt hinzufügen"** → Name z.B. `haushalt` → Weiter
3. Google Analytics kannst du **ausschalten** (nicht nötig) → **Projekt erstellen**

## 2. Datenbank (Firestore) anlegen
1. Linkes Menü → **Build → Firestore Database** → **„Datenbank erstellen"**
2. Standort: `eur3 (Europe)` → Weiter
3. Modus: **„Im Testmodus starten"** wählen → Aktivieren
   *(Reicht für den Start. Siehe Schritt 5 für die Dauer-Regel.)*

## 3. Web-App registrieren & Zugangsdaten holen
1. Projekt-Übersicht (Zahnrad oben links → **Projekteinstellungen**)
2. Runterscrollen zu **„Meine Apps"** → auf das **`</>`**-Symbol (Web) klicken
3. Spitzname z.B. `haushalt-web` → **App registrieren** (Hosting NICHT nötig)
4. Es erscheint ein Code-Block `const firebaseConfig = { ... }` — **das sind deine Werte**

## 4. Werte in die App eintragen
Öffne `index.html`, ganz oben im `<script>` steht:

```js
const FIREBASE_CONFIG = {
  apiKey: "",
  authDomain: "",
  projectId: "",
  storageBucket: "",
  messagingSenderId: "",
  appId: ""
};
```

Trag dort die Werte aus Firebase ein (copy/paste die Strings in die Anführungszeichen).

## 5. Sicherheitsregel setzen (wichtig, damit es dauerhaft läuft)
Firestore → Reiter **„Regeln"** → den Inhalt ersetzen durch:

```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /families/{code} {
      allow read, write: if true;
    }
  }
}
```
→ **Veröffentlichen**.

> Hinweis: Diese Regel erlaubt Lese-/Schreibzugriff auf alle, die deinen
> Familien-Code kennen. Für eine private Familien-App völlig okay. Nimm einen
> Code, der nicht zu erraten ist (z.B. `whittaker-7421`).

## 6. App benutzen
1. `index.html` auf jedem Gerät öffnen (per iCloud/AirDrop/Mail verteilen oder hosten)
2. Oben rechts **☁️ Sync** antippen → **Familien-Code** eingeben (auf allen Geräten **denselben**!)
3. Speichern → 🟢 = verbunden. Fertig — Punkte syncen jetzt in Echtzeit.

## App aufs iPhone als „echte App"
- `index.html` in **Safari** öffnen → **Teilen-Symbol** → **„Zum Home-Bildschirm"**
- Eigenes Icon, Vollbild, kein Browser-Rand. Kostenlos, kein App Store.

## Damit die Datei online erreichbar ist (optional, gratis)
Eine einzelne HTML-Datei kannst du gratis hosten, z.B.:
- **Netlify Drop** (https://app.netlify.com/drop) — Datei reinziehen, fertige URL
- **GitHub Pages** — Repo + Pages aktivieren
- **Firebase Hosting** — schon im selben Projekt vorhanden

Dann öffnen alle Geräte einfach diese URL.
