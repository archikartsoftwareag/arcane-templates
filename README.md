# Archikart Templates für Arcane

_TESTING_ - noch nicht für den produktiven Einsatz freigegeben

Template-Registry der Archikart Software AG (ALKIS, Elastic) für [Arcane](https://getarcane.app).

## Arcane installieren (empfohlen)

[`arcane/compose.yaml`](arcane/compose.yaml) startet Arcane und richtet beim Hochfahren automatisch ein:

- die Archikart Template-Registry (diese Registry)
- die Archikart Docker-Registry `docker.archikart.de` inkl. Benutzer/Passwort (wenn angegeben)

Es gibt **keine Pflichtvariablen**. Beim ersten Start erzeugt der Hilfscontainer `arcane-keys` den
`ENCRYPTION_KEY` und einen Admin-API-Key und legt beide im Volume `arcane-secrets` ab (werden nie überschrieben).
`arcane-init` nutzt den API-Key für die Einrichtung. Beide Hilfscontainer beenden sich danach
wieder (Status „Exited (0)“ ist korrekt). Bereits vorhandene Einträge werden nicht verändert.

Die Zugangsdaten zu `docker.archikart.de` stehen nicht im Repository (siehe Handbuch). Sie können beim Deploy
angegeben oder jederzeit in Arcane unter Customization → Container Registries eingetragen werden.

Optionale Variablen (Details in [`arcane/.env.example`](arcane/.env.example)):

| Variable | Beschreibung |
|---|---|
| `DOCKER_REGISTRY_USER` / `DOCKER_REGISTRY_PASSWORD` | Zugang zu `docker.archikart.de`. Leer = Registry wird nicht eingetragen. |
| `APP_URL` | Aufruf-URL von Arcane, z. B. `http://dockerhost:3552` |
| `ENCRYPTION_KEY` | Nur zur Übernahme einer bestehenden Installation; wirkt nur beim allerersten Start. |

**Sicherung:** Volumes `arcane-data` und `arcane-secrets` gemeinsam sichern. Geht `arcane-secrets` verloren,
müssen die in Arcane gespeicherten Passwörter (z. B. Registry-Zugang) neu eingetragen werden.

### Variante A: Portainer App-Template

Wenn die [Archikart Portainer-Templates](https://github.com/archikartsoftwareag/portainer-templates) eingebunden
sind: App Templates → **Arcane** → optional Registry-Zugangsdaten eintragen → **Deploy the stack**.

Ohne App-Template: Stacks → **Add stack** → Name `arcane` → **Repository**:

- Repository URL: `https://github.com/archikartsoftwareag/arcane-templates`
- Repository reference: `refs/heads/main`
- Compose path: `arcane/compose.yaml`

### Variante B: Kommandozeile

```sh
mkdir -p /opt/arcane && cd /opt/arcane
curl -fsSLO https://raw.githubusercontent.com/archikartsoftwareag/arcane-templates/main/arcane/compose.yaml
docker compose up -d
```

Registry-Zugangsdaten danach in Arcane eintragen oder vor dem Start in eine `.env` neben der `compose.yaml`
schreiben (`DOCKER_REGISTRY_USER=…`, `DOCKER_REGISTRY_PASSWORD=…`).

Arcane ist danach unter `http://<host>:3552` erreichbar (Login `arcane` / `arcane-admin`, Passwortwechsel wird erzwungen).

## Registry manuell in Arcane hinzufügen

Nur nötig, wenn Arcane nicht über `arcane/compose.yaml` installiert wurde.
Customization → Templates → **Add Registry**:

```text
https://raw.githubusercontent.com/archikartsoftwareag/arcane-templates/main/registry.json
```

Danach erscheinen die Archikart-Stacks in der Template-Galerie: Stack auswählen →
„Create Project" → Einstellungen im vorgefüllten `.env` anpassen → deployen.

Hinweis: Arcane lädt Template-Registries nur von öffentlich erreichbaren Adressen
(private/interne IP-Adressen werden blockiert).

## Repository-Aufbau

- `arcane/`: Compose zur Installation von Arcane selbst (inkl. automatischer Einrichtung)
- `templates/<id>/`: `template.json`, `compose.yaml`, `.env.example`, `README.md`
- `registry.json`: zentrale Registry-Datei (Schema: https://registry.getarcane.app/schema.json)
- `content_hash`: SHA-256 über `template.json`, `compose.yaml`, `.env.example`, `README.md` gemäß [offiziellem Registry-Generator](https://github.com/getarcaneapp/templates/blob/main/scripts/lib/registry.ts) – wird von Arcane selbst nicht ausgewertet, nur zur Konsistenz mit dem offiziellen Format
- Quelle der Compose-Files: [portainer-templates](https://github.com/archikartsoftwareag/portainer-templates) (dort einzige Quelle für die Compose-Inhalte)

## Pflege bei neuen Archikart-Versionen

1. Neues Compose + Template-Eintrag im [portainer-templates](https://github.com/archikartsoftwareag/portainer-templates)-Repo
2. Neues Template-Verzeichnis `templates/<id>/` hier anlegen (Compose übernehmen, `template.json`, `.env.example`, `README.md`)
3. `registry.json` um den Eintrag ergänzen (`content_hash` neu berechnen)
4. Committen/puschen – Kunden müssen nichts tun (Arcane lädt die Registry spätestens nach 5 Minuten neu)
5. Es werden nur die aktuellen Versionen (derzeit 4.79–4.81) gepflegt; ältere Einträge aus `registry.json` und `templates/` entfernen

## Lizenz

Siehe [LICENSE](LICENSE).
