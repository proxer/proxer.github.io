---
layout: post
title:  "Neue Proxer-Mails"
date:   2023-09-07 16:30:00 +0200
categories: devblog mails
author: Dravorle
---

Seit etwas über 3 Wochen haben wir die verpflichtende Verifikation bzw. Aktualisierung der Mail-Adressen eingeführt. Genauere Infos findet ihr in [Genesis' Update-Beitrag](/devblog/updates/2023/08/21/updates-august.html). In dieser Zeit haben wir unzählige Mails versendet und eins ist uns dabei ein Dorn im Auge gewesen: Die Mails waren Plain-Text und oft sogar Standardtexte von Joomla-Plugins.

### Neue Mail-Templates

Es wurde also Zeit ein neues Template zu erstellen, damit wir sowohl ein wenig unsere Brand vertreten können als auch einfach eine schönere Ansicht haben.
Obwohl ich wusste, dass Mail-HTML sehr in der Zeit zurück liegt, war ich ein wenig geschockt, wie schlimm es wirklich war. Im Endeffekt habe ich mich dafür entschieden, dass wir die Mails mit [React-Email](https://github.com/resendlabs/react-email) zu erstellen. Später erlaubt das eine einfachere Einbindung in die neue Codebase und bis dahin können wir die Mails ganz einfach zu HTML exportieren und in der PHP-Codebase importieren.

### Das vorläufige Ergebnis
<img src="https://cdn.proxer.me/f/A6WbemMN" alt="Password vergessen-Mail" width="400" style="inline"/><img src="https://cdn.proxer.me/f/XrBVddG1" alt="Neuer Account-Mail" width="400" style="inline"/>

Wir sind vorerst zufrieden mit dem Ergebnis und belassen es dabei. Wir haben es in mehreren verschiedenen Clients getestet und überall dort, wo es nicht ordentlich gerendert wird, funktioniert die normale Text-Only-Funktion zumindest wunderbar.