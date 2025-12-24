# 🔍 New Domain Check

Automatisierte Domain-Überwachung mit GitHub Actions. Dieses Repository überwacht Domains, die häufig ihre Top-Level-Domain (TLD) ändern, und hält dich über Änderungen auf dem Laufenden.

## 📋 Übersicht

Viele Websites ändern regelmäßig ihre Domain-Endungen (z.B. von `.do` zu `.lol` zu `.com`). Dieses Tool:
- ✅ Überprüft automatisch die aktuelle Domain
- ✅ Erkennt Redirects und Domain-Änderungen
- ✅ Speichert die Historie aller Domain-Änderungen
- ✅ Läuft alle 5 Minuten über GitHub Actions
- ✅ Commitet Änderungen automatisch ins Repository

## 🚀 Features

- **Automatische Überprüfung**: Läuft alle 5 Minuten via GitHub Actions Cron-Job
- **Redirect-Erkennung**: Folgt HTTP-Redirects und extrahiert die finale URL
- **Domain-Historie**: Speichert alle Domain-Änderungen mit Zeitstempel in `megakino-domain-history.txt`
- **Aktuelle Domain**: Die aktuell aktive Domain wird in `current-megakino-domain.txt` gespeichert
- **Automatische Commits**: Änderungen werden automatisch ins Repository gepusht

## 📂 Dateistruktur

```
.
├── .github/
│   └── workflows/
│       └── check-megakino-domain.yml    # GitHub Actions Workflow
├── current-megakino-domain.txt          # Aktuelle Domain
├── megakino-domain-history.txt          # Historie aller Änderungen
└── README.md                            # Diese Datei
```

## ⚙️ Wie es funktioniert

1. **Workflow-Trigger**: Der GitHub Actions Workflow wird alle 5 Minuten ausgeführt (`cron: '*/5 * * * *'`)
2. **Domain auslesen**: Die aktuelle Domain wird aus `current-megakino-domain.txt` gelesen
3. **Redirect-Check**: Ein HTTP-Request folgt allen Redirects zur finalen URL
4. **Vergleich**: Die finale Domain wird mit der gespeicherten Domain verglichen
5. **Bei Änderung**:
   - Die neue Domain wird in `current-megakino-domain.txt` gespeichert
   - Ein Eintrag mit Zeitstempel wird zur Historie hinzugefügt
   - Die Änderungen werden automatisch committed und gepusht

## 🛠️ Anpassung für eigene Domains

### 1. Repository forken

Forke dieses Repository zu deinem eigenen GitHub Account.

### 2. Domain ändern

Bearbeite `current-megakino-domain.txt` und trage deine zu überwachende Domain ein:

```bash
echo "deine-domain.com" > current-megakino-domain.txt
```

### 3. Workflow anpassen (optional)

Bearbeite `.github/workflows/check-megakino-domain.yml` und passe folgende Werte an:

- **Workflow-Name**: Zeile 1
- **Cron-Schedule**: Zeile 4 (Standard: alle 5 Minuten)
- **Dateinamen**: Falls du andere Dateinamen verwenden möchtest

### 4. Permissions setzen

Stelle sicher, dass der Workflow Schreibrechte hat:

1. Gehe zu **Settings** → **Actions** → **General**
2. Unter **Workflow permissions** wähle **Read and write permissions**
3. Aktiviere **Allow GitHub Actions to create and approve pull requests**

## 📊 Beispiel-Output

### current-megakino-domain.txt
```
megakino.lol
```

### megakino-domain-history.txt
```
2025-12-24T13:45:23 UTC | megakino.do | megakino.lol
2025-12-20T09:12:45 UTC | megakino.com | megakino.do
2025-12-15T14:30:11 UTC | megakino.net | megakino.com
```

## 🔧 Technische Details

### GitHub Actions Workflow

Der Workflow verwendet:
- **Ubuntu Latest** als Runner
- **Bash-Scripting** für die Domain-Checks
- **curl** mit `--location` Flag zum Folgen von Redirects
- **Git-Automatisierung** für automatische Commits

### Redirect-Verfolgung

```bash
curl --silent --location --output /dev/null --write-out "%{url_effective}" "$DOMAIN"
```

Dieser Befehl:
- Folgt allen HTTP-Redirects (`--location`)
- Gibt die finale URL aus (`--write-out`)
- Verwirft die Response-Body (`--output /dev/null`)

## 🤝 Contributing

Beiträge sind willkommen! Erstelle gerne Issues oder Pull Requests für:
- Bug-Fixes
- Neue Features
- Dokumentations-Verbesserungen
- Zusätzliche Domain-Checks

## 📝 Lizenz

Dieses Projekt steht unter der MIT License.

## ⚠️ Hinweis

Dieses Tool ist für Monitoring-Zwecke gedacht. Bitte respektiere die Robots.txt und Terms of Service der überwachten Websites.

---

**Erstellt mit ❤️ und GitHub Actions**
