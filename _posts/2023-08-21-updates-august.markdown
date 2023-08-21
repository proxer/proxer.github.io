---
layout: post
title:  "Updates August 2023"
date:   2023-08-21 15:00:00 +0200
categories: devblog updates
author: Genesis
---

Proxer ist für uns eine Tätigkeit, die wir in unserer Freizeit durchführen. Ich habe seit letzter Woche Urlaub und habe ich mir vorgenommen, ein wenig meine angestauten Aufgaben auf Proxer anzugehen. Es sind dabei ein paar Neuerungen dazu gekommen, auf die ich hier kurz eingehen möchte.

### Entwickler Blog und GitHub

Wie Dravorle im [vorherigen Post](/2023/08/15/first-devblog.html) erläutert hat, haben wir nun ein Entwickler-Blog! Das Ganze ist auf GitHub gehostet. Im Hintergrund arbeiten wir daran, öfter GitHub für unsere Entwicklungsaktivitäten zu nutzen. Wir möchten beispielsweise mehr GitHub Actions nutzen, um unsere Herausforderungen zu meistern. Wir versprechen uns so eine modernere Infrastruktur, ohne dabei neue Baustellen oder Infrastruktur aufzubauen.

### E-Mail Bestätigungen

Seit 2021 wurden E-Mail Adressen nur bei der Registrierung und bei Änderung der E-Mail Adressen bestätigt. Für die Änderung der E-Mail Adressen war bislang nur das Passwort nötig und für mehrere Jahre vor 2021 konnte man sich auf Proxer sogar ohne E-Mail Bestätigung registrieren. Das heißt, dass ein großer Teil unserer Nutzer ungeprüfte oder gar falsche Mailadressen nutzen. Schreibfehler passieren erstaunlich oft bei E-Mail Adressen.

Mit Fortschreiten der Zeit werden Cyberangriffe ausgeklügelter. Der Schutz von Identitäten erfordert besondere Aufmerksamkeit und die Zwei-Faktor-Authentisierung spielt hier eine entscheidende Rolle. Auf Proxer gibt es bereits die Zwei-Faktor Authentisierung seit über sechs Jahren. Nutzer haben die Möglichkeit, die Authentisierung via Mail oder via Token (OTP) auszuwählen. Das alles ist aber bislang freiwillig.

Wir wollen bereits seit Jahren die Sicherheit auf Proxer erhöhen, indem wir die Zwei-Faktor-Authentisierung verpflichtend machen und zumindest mal über E-Mail den Login abprüfen (falls man sich beispielsweise zum ersten Mal mit einem bestimmten IP-Netz einloggt). Das hat sich aber als schwieriger herausgestellt als erwartet. Als wir das einmal kurz aktiviert hatten, wurde unser Support Postfach gefüllt von E-Mails, bei denen unsere Mitglieder nicht mehr auf ihre Accounts zugreifen konnte. Es war also ein Mechanismus nötig, um alle existierenden E-Mail Adressen zu bestätigen und zukünftig keine Änderung ohne Bestätigung der E-Mail Adressen zu erlauben.

Seit letzter Woche werden Mitglieder mit einer Benachrichtigung und Prüfung der Mailadressen benachrichtigt. Dieses Update dient als Vorbereitung für weitere Updates rund um Identitäten und Sicherheit.

### Proxer Stream Load Balancing

Unsere Proxer-Stream Infrastruktur (rotes Symbol bei der Episodenwahl) hatte vor ein paar Monaten eine der größten Veränderungen seit Jahren erlebt. Es hatte zur Folge, dass wir nun eine modernere, wartbare und auch noch günstigere Infrastruktur zur Verfügung haben, mit dem wir alle Anforderungen von Proxer langfristig abdecken können. In der alten Infrastruktur war Load-Balancing eine große Herausforderung.

Ich habe mir vor kurzem Zeit genommen, das Load Balancing zu automatisieren, damit unsere Proxer-Streams langfristig die Lastspitzen abdecken kann. Das Update war am Ende mit existierenden Werkzeugen sehr einfach umzusetzen. Ergänzend zu unseren blauen Proxer-Streams sind die roten Proxer-Streams nun ebenfalls gerüstet für weitere Jahre.

### Stream-Bot Encoding

Unsere Streams werden überwiegend automatisiert über unsere spezialisierte Pipeline heruntergeladen, umgewandelt (encodet) und wieder hochgeladen. Das Umwandeln bedeutet, dass die Videos in so ein Format umgewandelt werden, dass sie effizient von Endgeräten konsumiert werden können. In unserem Fall ist es die Auswahl der korrekten Audio- und Untertitelspur und Export als ein sogenanntes „Hardsub“.

Video Encoding ist eine komplizierte Geschichte und bisher war es so, dass automatisch immer die erste Audio- und Untertitelspur ausgewählt wurde. Das hatte zur Folge, dass die Implementierung zwar einfach und in 90% der Fälle funktioniert hat, aber in einigen Fällen, in denen Videos mehrspurige Audios und Untertitel haben, fehl geschlagen hat. Das ist besonders bei Anime-Filmen der Fall gewesen.

Das Update umfasst, dass nun eine beliebige Audio- oder Untertitelspur ausgewählt werden kann. Dadurch sollten wir auf Proxer künftig des öfteren Filme vorfinden, die bislang aufgrund von fehlender Funktionalität und Mangel an Kapazitäten nicht hochgeladen wurden.

### Vorschau auf nächste Projekte

Einer meiner nächsten großen Aufgaben ist das Monitoring und Alerting auf Proxer von Grund auf neu aufzubauen. Unser Server-Monitoring basiert bislang auf Munin, was zwar eine sehr einfache und stabile Lösung ist, doch mit ein paar Limitierungen kommt. So ist es damit nicht möglich Benachrichtigungen auszulösen, wenn bestimmte Bedingungen (wie Serverausfälle) ausgelöst werden.

Des weiteren müssen wir in der Lage sein, nicht nur Server-Monitoring durchzuführen, sondern auch auf Anwendungsebene eine Art Monitoring einzuführen. Wenn eine Anwendung bestimmte Fehler oder Bedingungen auslöst, sollen wir in einer zentralen Übersicht über alle Gegebenheiten informiert werden. Bislang steht Grafana und Prometheus dafür auf dem Plan.

Habt ihr Feedback zu unserem Blog und oder zu unseren Projekten? Nutzt gerne unsere [GitHub Issues](https://github.com/proxer/proxer.github.io/issues) Seite, um dies uns mitzuteilen!
