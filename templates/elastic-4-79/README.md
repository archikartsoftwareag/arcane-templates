# Archikart Elastic 4.79

Archikart Elastic 4.79 Ermöglicht die Nutzung der Globalen Suche im AK4 mit Elastic. Der Name des zugehörigen Volumes (Speicherort der Elastic Daten für die Suche) ist elasticsearch, wobei diesem der Name des Stacks vorangeht. Beispiel ak_elastic_elasticsearch. Bitte beachten Sie das eine latest Version von uns zur Zeit nicht bereitgestellt wird. Alle Felder müssen ausgefüllt sein. Für alle Archikart 4.79 Versionen.

## Images

- docker.archikart.de/archikart/elasticsetup:4.79
- docker.archikart.de/archikart/elasticsearch:4.79
- docker.archikart.de/archikart/logstash:4.79

## Umgebungsvariablen

| Variable | Default | Beschreibung |
|---|---|---|
| ELASTIC_PASSWORD | **(Pflichtfeld)** | Passwort für die Authentifizierung mit Elastic. Wird auch bei der Anmeldung im AK4 Elastic Service benötigt. |
| ELASTIC_PORT | 9200 | Über diesen Port ist der Elastic Service erreichbar. Default ist 9200. |
| LOGSTASH_PORT | 9201 | Über diesen Port ist der Logstash Service erreichbar. Default ist 9201. |
| ELASTIC_JVM_MEMORY | 1 | Bestimmt die Arbeitsspeichermenge in GByte der Elastic JVM, sollte mindestens 1 sein. |
| ELASTIC_CONTAINER_MEMORY | 2 | Bestimmt die maximale Arbeitsspeichermenge in GByte des Elastic Container, sollte mindestens ELASTIC_JVM_MEMORY + 1 sein. |
| LOGSTASH_JVM_MEMORY | 2 | Bestimmt die Arbeitsspeichermenge in GByte der Logstash JVM, sollte mindestens 2 sein. |
| CRON_SCHEDULE | 0 2 * * * | Bestimmt die Zeit zu welcher das Elastic alle Daten Neu einliest. |
| NETWORK_NAME | archikartnet | Name des internen Docker Netz. Nur Container im gleichen Netzwerk können miteinander kommunizieren. |

## Hinweise

- Für Kunden ohne Internet auf dem Docker-Host (Image-Transfer): Abschnitt „Kunden ohne Internet“ in der [portainer-templates-README](https://github.com/archikartsoftwareag/portainer-templates).
