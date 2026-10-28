# Archikart ALKIS 4.79

Archikart ALKIS 4.79 Für ALKIS ab 4.79.0.

## Images

- docker.archikart.de/archikart/alkis_gid7:4.79
- docker.archikart.de/archikart/alkis_postgis:4.79

## Umgebungsvariablen

| Variable | Default | Beschreibung |
|---|---|---|
| ALKIS_POSTGIS_PORT | 5432 | Der Port für die Kommunikation mit dem Datastore. Ändern Sie ggf. den gesetzten default Port. |
| ALKIS_GID7_PORT | 10722 | Der Port für die Kommunikation mit der ALKIS Instanz. Ändern Sie ggf. den gesetzten default Port. |

## Hinweise

- Für Kunden ohne Internet auf dem Docker-Host (Image-Transfer): Abschnitt „Kunden ohne Internet“ in der [portainer-templates-README](https://github.com/archikartsoftwareag/portainer-templates).
