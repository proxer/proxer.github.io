---
layout: post
title:  "Statusseite mit Cachet"
date:   2023-10-27 14:30:00 +0200
categories: devblog services
author: genesis
---

Die Proxer.Me Statusseite gibt seit einem Jahrzehnt unseren Mitgliedern einen Überblick über den aktuellen Status unserer Server. In den herausforderndsten Zeiten, wenn bei uns die Server wegen Angriffen oder Überlastung nicht verfügbar sind, ist die Statusseite die erste Anlaufstelle, um Informationen abzufragen.
Die bisherige Statusseite war ein wenig in die Jahre gekommen und hat nicht mehr unsere Anforderungen erfüllt. Es ist die Zeit gekommen, ein Relaunch unserer Statusseite anzugehen! 

### Anforderungen

Ein großes Problem unserer bisherigen Statusseite war, dass sie nicht in ausreichendem Maße die Ausfälle abgefangen und dargestellt hat. Es gab auf der alten Statusseite nur dann ein "Ausfall", wenn der Server vollständig nicht erreichbar war. Unsere Infrastruktur ist inzwischen viel stabiler geworden und viel relevanter sind nun Informationen über Leistungsproblemen oder Timeouts. 

Einfachheit ist eine wichtige Anforderung, die wir auf Proxer wert schätzen. Eine einfache Lösung erlaubt eine gute Wartbarkeit und dass wir uns möglichst auf die entscheidenderen Herausforderungen fokussieren können.

### Statusseiten mit Cachet

Das [Open-Source Statusseitensystem Cachet](https://github.com/cachethq/cachet) ist ein einfaches, und doch mächtiges Werkzeug, um Statussseiten aufzusetzen. Die Einrichtung erfolgt schnell. In Cachet entspricht ein Server einer Komponente (z.B. "Stream 14") und man kann Komponenten zu Komponentengruppen (z.B. "Proxer Stream") zusammenfassen. Cachet bietet von Haus aus die Möglichkeit an, manuell oder über die API den Status von Komponenten zu aktualisieren.

Die Möglichkeit über eine API den Status zu aktualisieren ist genau das, was wir für Proxer brauchen. Wir haben ein komplexes Geflecht an Systemen, auf denen unterschiedlichste Anwendungen laufen. Die Messung für "Leistungsprobleme" und "Ausfälle" unterscheiden sich je nach System. Am besten kann man den Status abfragen, indem man die nötigen Daten sammelt, aggregiert und mithilfe der API den Status aktualisiert.

### Nyan-Cat

<img src="https://cdn.proxer.me/f/ATxbbMu0" alt="Nyan-Cat" />

Das Nyan-Cat Design der alten Statusseite hat uns über die Jahre mit begleitet. Es ist nahezu ikonisch geworden und teil unserer Proxer-Kultur. Daher wurde das Design auch in Cachet übernommen, was sich als eine gut machbar gezeigt hat.

### Datenquelle

Auf unseren Proxer-Streams haben wir bereits seit längerem ein automatisches Meldesystem, falls unsere Mitglieder mit Timeouts konfrontiert sind. Wir können also genau sehen, wann welcher Server irgendwelche Probleme hatte.
Die Daten aus diesen automatischen Meldungen geben aufschluss über den Status der Stream-Server. Haben wir in kurzer Zeit ein paar Meldungen, so ist der Server überlastet. Haben wir sehr viele Meldungen, so ist ein Server vermutlich ausgefallen.

Für [Python hat Cachet eine Bibliothek](https://pypi.org/project/cachet-client/), mit der man die Cachet API ansteuern kann. Mithilfe dieser Bibliothek ist es nur ein paar Zeilen Code, den Status der Server zu aktualisieren. 

```python
client = cachetclient.Client(
    endpoint='https://status.proxer.de/api/v1',
    api_token='API_TOKEN',
)

component = client.components.update(
    component_id=COMPONENT_ID,
    status=enums.COMPONENT_STATUS_PERFORMANCE_ISSUES
)
```

Für die Bestimmung des Serverstatus der Streamserver unterstützt uns beispielsweise der folgende Code-Baustein:
```python
# Iterate through the server array
for server in server_array:
    hostname = server["hostname"]
    component_id = server["component_id"]

    try:
        # Send an HTTP request to the server with a 10-second timeout
        response = requests.get(hostname, timeout=10, verify=False)

        # Check the HTTP status code and map it to a Cachet component status
        status_code = response.status_code
        component_status = status_mapping.get(status_code, enums.COMPONENT_STATUS_MAJOR_OUTAGE)
        
        if component_status == enums.COMPONENT_STATUS_OPERATIONAL and server.get('reports'):
            reports = int(server.get('reports'))
            if reports > 10 and reports < 20:
                component_status = enums.COMPONENT_STATUS_PERFORMANCE_ISSUES
            elif reports >= 20 and reports < 50:
                component_status = enums.COMPONENT_STATUS_PARTIAL_OUTAGE
            elif reports >= 50:
                component_status = enums.COMPONENT_STATUS_MAJOR_OUTAGE
        # Update the component status in Cachet
        component = client.components.update(
            component_id=component_id,
            status=component_status
        )

        print(f"Updated {server['server_name']} ({hostname}) to status: {component_status}")
    
    except requests.RequestException as e:
        component_status = enums.COMPONENT_STATUS_PERFORMANCE_ISSUES
        component = client.components.update(
            component_id=component_id,
            status=component_status
        )
        
        # Handle exceptions (e.g., timeout, network error)
        print(f"Failed to update {server['server_name']} ({hostname}): {str(e)}")
```

Ist nicht perfekt, aber ausreichend. Die Konstanten haben sich mit unseren Tests ergeben. Wir wollen weder von Meldungen überschwemmt werden, noch uninformiert bleiben über Ausfälle. Wenn die Anzahl der Meldungen der letzten zwei Stunden bestimmte Werte überschreiten, so wird der Status entsprechend angepasst. Falls wir ein Timeout bei HTTP Abfrage haben, so ist sehr wahrscheinlich der Server gerade überlastet. Das Python Script wird über ein Cron-Job in regelmäßigen Zeitabständen angestoßen.

### Ergebnis

Die Zeiten instabiler Server haben wir hinter uns. Wenn aber dennoch wieder einmal die Server brennen und etwas nicht läuft, dann ist die erste Anlaufstelle die Statusseite. 

<img src="https://cdn.proxer.me/f/wmgKXFqk" alt="Ansicht der Statusseite" />

[Die neue Statusseite](https://status.proxer.me) vermittelt jederzeit eine Übersicht über den aktuellen Status unserer Server. Unsere Mitglieder können Statusmeldungen abonnieren und können gar über E-Mail die Information erhalten.
