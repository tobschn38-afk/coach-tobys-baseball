# Coach Tobys Baseball App – Weg in den App Store und zu Google Play

Dieses Projekt verpackt die Web-App mit **Capacitor** in eine native App für iPhone und Android.
Der gesamte App-Inhalt liegt in `www/index.html`. Die App läuft offline, braucht keinen Server und sammelt keine Daten.

```
coach-tobys-baseball/
├── www/index.html          die App selbst
├── assets/                 Quellbilder für App-Icon und Startbildschirm
├── capacitor.config.json   App-Name, App-ID, Farben
├── package.json            Abhängigkeiten und Befehle
├── STORE-TEXTE.md          Texte für die Store-Einträge
└── DATENSCHUTZ.md          Datenschutzerklärung (muss online gestellt werden)
```

---

## 0. Vorher klären

**App-ID.** In `capacitor.config.json` steht `de.coachtoby.baseball`. Die ID muss weltweit eindeutig sein und kann nach der Veröffentlichung nicht mehr geändert werden. Ideal ist eine umgekehrte Domain, die dir gehört, z. B. `de.<deinedomain>.baseball`.

**Kosten und Konten.**
- **Apple Developer Program:** 99 € pro Jahr. Gemeinnützige Organisationen können sich davon befreien lassen. Ein eingetragener, gemeinnütziger Verein kann das bei der Registrierung beantragen (nur für kostenlose Apps ohne In-App-Käufe). Für ein Organisationskonto braucht ihr eine D-U-N-S-Nummer, die ist kostenlos.
- **Google Play Console:** einmalig 25 US-Dollar.
  Achtung: Bei einem **privaten** Konto muss die App vor der Veröffentlichung 14 Tage lang mit mindestens 12 Testern im geschlossenen Test laufen. Bei einem **Organisationskonto** (mit D-U-N-S-Nummer) entfällt das. Wer kein Unternehmen oder keinen Verein im Rücken hat, muss den Testzeitraum einplanen.

**Hardware.**
- Für Android reicht jeder Windows-, Mac- oder Linux-Rechner mit **Android Studio**.
- Für iOS braucht ihr zwingend einen **Mac mit Xcode**.

---

## 1. Einmalige Einrichtung

1. **Node.js** (Version 20 oder neuer) installieren: https://nodejs.org
2. Im Ordner `coach-tobys-baseball` ein Terminal öffnen und ausführen:

```bash
npm install
npx cap add android
npx cap add ios          # nur auf dem Mac
npm run icons            # erzeugt alle Icon- und Splash-Größen aus assets/
npx cap sync
```

---

## 2. Android (Google Play)

1. `npm run android` öffnet das Projekt in Android Studio.
2. Auf einem Handy oder Emulator testen (grüner Play-Button).
3. **Build → Generate Signed App Bundle** → *Android App Bundle (.aab)*.
   Dabei einen neuen Keystore anlegen. **Die Keystore-Datei und das Passwort sicher aufbewahren.** Ohne sie lassen sich keine Updates mehr veröffentlichen.
4. In der Play Console eine neue App anlegen, die `.aab` hochladen und die Texte aus `STORE-TEXTE.md` eintragen.
5. Pflichtangaben in der Play Console:
   - **Datensicherheit:** „Es werden keine Daten erhoben oder geteilt.“
   - **Zielgruppe:** Wenn auch Kinder unter 13 die App nutzen sollen, die Richtlinien für Familien beachten. Die App enthält keine Werbung, kein Tracking und keine Käufe.
   - **Inhaltsfreigabe:** Fragebogen ausfüllen, Ergebnis ist „Alle Altersgruppen“ bzw. USK 0.
   - **Datenschutzerklärung:** URL zu einer Online-Version von `DATENSCHUTZ.md`.

---

## 3. iOS (Apple App Store)

1. `npm run ios` öffnet das Projekt in Xcode.
2. Unter *Signing & Capabilities* das Team, also das Entwicklerkonto, auswählen.
3. Auf einem iPhone testen.
4. **Product → Archive** → *Distribute App* → *App Store Connect*.
5. In App Store Connect die App anlegen, Texte und Screenshots eintragen:
   - Screenshots für 6,9″ (iPhone) und, falls iPad unterstützt wird, 13″
   - **App-Datenschutz:** „Keine Daten erhoben“
   - **Altersfreigabe:** 4+
6. Zur Prüfung einreichen.

**Hinweis zur Apple-Prüfung (Richtlinie 4.2, Mindestfunktionalität):** Apple lehnt Apps ab, die nur eine Website anzeigen. Coach Tobys Baseball App läuft vollständig lokal, offline und ist interaktiv. Das solltet ihr im Feld *Notizen für die Prüfung* kurz erwähnen. Ein Textvorschlag steht in `STORE-TEXTE.md`.

---

## 4. Updates

Die App ändern, also `www/index.html` ersetzen, dann:

```bash
npx cap sync
```

Danach die Versionsnummer erhöhen:
- Android: `versionCode` und `versionName` in `android/app/build.gradle`
- iOS: *Version* und *Build* in Xcode

Anschließend neu bauen und hochladen wie oben.

---

---

## APK ohne eigene Installation bauen (GitHub, kostenlos)

GitHub baut die APK in der Cloud. Du brauchst nur ein kostenloses GitHub-Konto.

1. Auf github.com anmelden → oben rechts **+** → **New repository** → Name z. B. `coach-tobys-baseball` → **Private** → **Create repository**.
2. Auf der neuen Seite **uploading an existing file** anklicken und den **Inhalt** des Ordners `coach-tobys-baseball` hineinziehen (also `www`, `assets`, `package.json`, `capacitor.config.json` usw.) → **Commit changes**.
3. **Add file → Create new file**. Als Dateiname genau `.github/workflows/apk.yml` eintippen, den Inhalt der Datei `APK-WORKFLOW.yml` hineinkopieren → **Commit changes**.
4. Reiter **Actions** öffnen. Der Lauf „APK bauen“ startet automatisch und dauert etwa 5 bis 10 Minuten. Grüner Haken = fertig.
5. Den Lauf anklicken → unten bei **Artifacts** auf `coach-tobys-baseball-apk` klicken. Es wird eine ZIP-Datei mit `app-debug.apk` heruntergeladen.

**Auf dem Handy installieren:** `app-debug.apk` aufs Handy schicken (Mail, Google Drive, WhatsApp) und antippen. Android fragt einmal, ob Installationen aus dieser Quelle erlaubt sind → erlauben → **Installieren**.

Hinweise:
- Das ist eine Debug-APK. Sie ist zum Installieren und Weitergeben ans Team gedacht, nicht für den Play Store. Für den Store braucht es eine signierte Release-Version (siehe Abschnitt 2).
- Nach jeder Änderung an `www/index.html` (neue Datei hochladen) baut GitHub automatisch eine neue APK.
- iPhone: Für iOS gibt es keine APK. Dort bleibt der Weg über Xcode auf einem Mac oder die Web-App.

## 5. Screenshots erstellen

Die App im Simulator oder auf dem Handy öffnen und eine Situation mit gestopptem Ball und sichtbarer Frage aufnehmen. Gute Motive sind ein Groundball, ein Fly Ball mit Landepunkt und die Auflösung mit grüner Antwort.
