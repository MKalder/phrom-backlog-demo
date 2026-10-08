# Phrom Backlog Demo

> **Demo-Backlog für [Phrom](https://github.com/MKalder/phrom) (พร้อม)**
> _Eine schreibgeschützte CLI, die GitHub Issues vor dem Backlog Refinement prüft und Verbesserungsentwürfe erstellt, die der Product Owner reviewt._

[English version](README.md)

Dieses Repository enthält ausschließlich **synthetische Beispieldaten**: Epics, Stories, Tasks und Bugs für ein fiktives Self-Service-Kundenportal. Es ist das Test-Backlog für Phrom und enthält weder echten Produktcode noch vertrauliche Informationen.

Warum Code und Demo-Daten in getrennten Repositories liegen, beschreibt [ADR-002: Getrennte Repositories für Code und Demo-Backlog](https://github.com/MKalder/phrom/blob/main/adr/de/ADR-002-separate-repositories.de.md).

---

## Zweck

1. **Phrom ohne Risiko ausprobieren:** Phrom liest dieses Repository nur. Es braucht keinen Schreibzugriff und für ein öffentliches Repository nicht einmal einen Token.
2. **Ein festes Test-Backlog mit bekannten Schwächen:** Die Issues enthalten bewusst typische Lücken, etwa fehlende Akzeptanzkriterien, fehlenden Kontext oder zu große Items. Jeder Typ hat außerdem mindestens ein gutes Kontroll-Issue, das nicht bemängelt werden sollte.
3. **Refinement-Showcase:** `npm run demo` im Haupt-Repository zeigt an diesen Issues eine formale Vorprüfung, eine KI-Analyse und einen Vorher-Nachher-Entwurf.

Die erwarteten Befunde je Issue liegen nicht hier, sondern im Haupt-Repository (`seed/issues.json`). Dort vergleicht `node scripts/eval-seed.js` die Ergebnisse von Phrom mit diesen Erwartungen. Ein Vergleich verschiedener Modelle steht noch aus.

---

## Inhalt

19 offene Issues (Stand 2026-10-08):

- **#1 bis #18** stammen aus der Seed-Datei des Haupt-Repositories: 3 Epics, 7 Stories, 4 Tasks und 4 Bugs, jeweils mit dem Label `demo-seed`.
- **#19** „Test Issue without issue type“ wurde von Hand **ohne Typ-Label** angelegt, um die Typ-Erkennung von Phrom zu testen.

| # | Typ | Titel | Angelegt als | Ergebnis am 2026-10-08 |
| ---: | --- | --- | --- | --- |
| 1 | Epic | Invoice self-service | gutes Kontroll-Issue | 100/100 🟢 |
| 2 | Story | Download invoice as PDF | gutes Kontroll-Issue | 100/100 🟢 |
| 3 | Story | Improve login | Story aus einem Satz | 10/100 🔴 |
| 4 | Story | Reset password | Produkt und Zielgruppe fehlen | 80/100 🔴 |
| 5 | Story | Manage account settings | zu breit | 36/100 🔴 |
| 6 | Epic | Modernize the portal | schwaches Epic | 0/100 🔴 |
| 7 | Story | View invoice overview | gutes Kontroll-Issue | 100/100 🟢 |
| 8 | Story | Change payment method | Epic-Verweis fehlt | 90/100 🟢 |
| 9 | Task | Database migration to PostgreSQL 18 | schwacher Task | 10/100 🔴 |
| 10 | Bug | PDF download fails on mobile Safari | Akzeptanzkriterien fehlen | 52/100 🔴 |
| 11 | Story | Download invoice | zu wenige Akzeptanzkriterien | 90/100 🔴 |
| 12 | Epic | Digital customer experience | zu breit | 41/100 🔴 |
| 13 | Task | Database migration to PostgreSQL 16 | gutes Kontroll-Issue | 100/100 🟢 |
| 14 | Task | Update the database | Task aus einem Satz | 0/100 🔴 |
| 15 | Task | Modernize infrastructure | zu breit | 50/100 🔴 |
| 16 | Bug | PDF download fails on mobile Safari (iOS 16) | gutes Kontroll-Issue | 100/100 🟢 |
| 17 | Bug | Download is broken | Bug aus einem Satz | 0/100 🔴 |
| 18 | Bug | Intermittent login failures across all platforms | komplexer Bug | 57/100 🔴 |
| 19 | – (Task, vom Modell erkannt) | Test Issue without issue type | kein Label | 15/100 🔴 |

Die Ergebnisse stammen aus einem `phrom run` am 2026-10-08 mit dem Modell `qwen3:30b-instruct` und Regelwerk 0.3.1. Mit einem anderen Modell oder einer anderen Regelwerksversion können sie abweichen.

- 🟢 heißt nur, dass ein Item die Kriterien des Regelwerks erfüllt. Eine Sprint-Zusage ist es nicht.
- #8 ist trotz fehlendem Epic-Verweis 🟢, weil `epic-link` optional ist.
- #4 und #11 sind trotz 80 und 90 Punkten 🔴, weil ein Pflichtkriterium fehlt.

---

## Labels

Phrom wählt anhand des Typ-Labels die passenden Kriterien:

| Label | Bedeutung | Was Phrom prüft |
| --- | --- | --- |
| `type:epic` | Große Initiative | Ziel, Nutzen, Liste der Stories oder Slices, Größenrisiko |
| `type:story` | User Story | Story-Format, Kontext, Akzeptanzkriterien und deren Testbarkeit, Nutzen, Größenrisiko |
| `type:task` | Technischer Task | Umfang, Begründung, Auswirkungen, Rollback-Plan, Machbarkeit |
| `type:bug` | Fehlerbericht | Reproduktionsschritte, erwartetes vs. tatsächliches Verhalten, Umgebung, Schweregrad |
| `demo-seed` | Seed-Kennzeichnung | Markiert Issues aus dem Seed-Skript; für die Bewertung ohne Bedeutung |

Fehlt das Typ-Label, ordnet das Modell einen Typ zu (Beispiel: #19). Diese Typ-Erkennung ist nicht evaluiert; vergib die Labels im eigenen Backlog daher selbst.

Das vollständige Regelwerk: [Phrom-Regelwerk (v0.3.1)](https://github.com/MKalder/phrom/blob/main/docs/RULES.de.md).

---

## Demo-Backlog verwenden

Dieses Repository ist öffentlich. Du brauchst keinen Schreibzugriff.

**Voraussetzungen:** Node.js, ein laufendes [Ollama](https://ollama.com) mit dem Modell `qwen3:30b-instruct` (etwa 19 GB Arbeitsspeicher) und das Phrom-Haupt-Repository.

1. **Haupt-Repository klonen und installieren:**

   ```bash
   git clone https://github.com/MKalder/phrom.git
   cd phrom
   npm install
   cp .env.example .env
   ```

2. **Diese Werte in `.env` setzen:**

   ```env
   GITHUB_OWNER=MKalder
   GITHUB_REPO=phrom-backlog-demo
   OLLAMA_HOST=http://localhost:11434
   MODEL_NAME=qwen3:30b-instruct

   # optional, hebt nur das GitHub-API-Limit an:
   #GITHUB_TOKEN=github_pat_your_token_here
   ```

   **Zum Token:** Für dieses öffentliche Repository ist kein Token nötig. Ohne Token erlaubt GitHub 60 API-Anfragen pro Stunde und IP-Adresse. Ein Demo-Lauf braucht etwa 25, der dritte Lauf innerhalb einer Stunde erreicht also das Limit. Jeder Token hebt das Limit auf 5.000 pro Stunde. Ein Fine-grained Token mit Lesezugriff auf öffentliche Repositories genügt. Phrom schreibt nie nach GitHub.

3. **Geführte Demo starten:**

   ```bash
   npm run demo
   ```

   Oder einzelne Befehle ausführen:

   ```bash
   npm run phrom status        # formale Vorprüfung, nur Regeln, wenige Sekunden
   npm run phrom select 3 7    # ausgewählte Issues inklusive KI bewerten
   npm run phrom improve 3     # bewerten und einen Verbesserungsentwurf erstellen
   npm run phrom run           # alle Issues bewerten (auf einem CPU-Server etwa 12 Minuten)
   ```

**Hinweis zu Entwürfen:** Verbesserungsentwürfe sind Vorschläge. In den Tests enthielten sie Details, die das Modell erfunden oder aus Referenzbeispielen übernommen hatte. Prüfe jeden Entwurf, bevor du einen Teil daraus verwendest.

---

## Wichtige Hinweise

- **Keine Issues oder Pull Requests hier:** Fehler und Wünsche zu Phrom bitte im Haupt-Repository melden: [github.com/MKalder/phrom/issues](https://github.com/MKalder/phrom/issues).
- **Seed und Reset:** Nur die getrennten Hilfsskripte im Haupt-Repository schreiben in dieses Backlog, mit eigenem Login und nie mit dem Analyse-Token von Phrom (siehe [ADR-004](https://github.com/MKalder/phrom/blob/main/adr/de/ADR-004-seed-and-reset-scripts.de.md)):
  - `npm run seed` legt fehlende Seed-Issues an und überspringt Issues, deren Titel schon existiert.
  - Ein separates Reset-Skript löscht die Issues; es braucht Admin-Rechte und eine ausdrückliche Bestätigung.
- **Issue-Nummern können sich ändern:** GitHub setzt den Issue-Zähler nicht zurück. Nach einem Reset beginnen neue Issues bei der nächsten freien Nummer, und Verweise wie „#3“ in Dokumentationen zeigen danach womöglich auf ein anderes Issue.

---

## Lizenz und Copyright

© 2026 Marius Kalder. Alle Rechte vorbehalten.
Dieses Testset wird ausschließlich zu Demonstrations- und Testzwecken in Verbindung mit [Phrom](https://github.com/MKalder/phrom) bereitgestellt.
