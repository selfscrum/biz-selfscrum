+++
title = "Home Office Video IT"
date = "2021-02-14T00:00:00"
image = "blog/green.jpg"
description = "Arbeitsplatzausstattung für virtuelle Zusammenarbeit"
disableComments = true
author = "martin-jahr"
+++

## Der virtuelle Arbeitsplatz ist ziemlich real

.

Für mich hat die Corona-Zeit arbeitsmäßig nicht viel Neues gebracht - auch vorher habe ich schon viel von Zuhause gearbeitet, gelegentlich in Videokonferenzen, meist per Telefonkonferenz und Screensharing.

Der Boom von Zoom und vergleichbaren andere Systemen wie Teams, Jitsi, BigBlueButton hat inzwischen jedoch die Kommunikation geändert. Nur selten wird noch telefoniert, meist wird gleich per Video Call gechattet. Daher habe ich mich kürzlich entschlossen, meinen Arbeitsplatz endlich so auszustatten, dass ich diese Arbeitsweise optimal unterstützen kann. Ich wollte

* Ein besseres Videobild
* Die Möglichkeit, virtuelle Hintergründe besser zu erzeugen als das die Software der Video-Meeting-Anbieter kann
* zwischen verschiedenen Eingangsquellen einfach umschalten bzw diese zusammenmischen
* Eine Möglichkeit, den so erzeugten Videostream aufzuzeichnen
* Einen Teleprompter
* Vernünftige Beleuchtung
* Die Möglichkeit, zwischen verschiedenen Aufnahmeszenarien schnell umschalten zu können

Klingt aufwändig! Am längsten hat die Suche nach Konzepten und den passenden Tools gedauert. Als ich diese zusammen hatte, ging das Setup erfreulich schnell voran. Noch nicht perfekt, aber ein guter Start, mit dem ich nun Erfahrungen sammeln werde. 

![home_office_it diagram](https://res.cloudinary.com/dzw4emsdt/image/upload/c_scale,w_900,q_auto/v1648042299/selfscrum/Portfolio-Digital_Consulting_gyjd5m.png)

Die Schaltung der Verbindungen ergibt sich aus dem Diagramm. Die Entscheidungen für die Verbindungstechnik ergeben sich aus der verfügbaren Hardware:

* Mein Windows-Notebook hat nur einen HDMI Ausgang. Zum Glück habe ich eine per USB-C anschließbare kleine Dockingstation, die neben einer RJ-45-Ethernetbuchse auch einen zweiten HDMI-Ausgang besitzt. Wichtig ist für die Videotechnik ein relativ leistungsstarkes System. Ich bin mit meinem Lenovo ThinkPad P1 Gen 3 leistungsmäßig sehr zufrieden, allerdings ist der Lüfter relativ laut und springt bei Video Processing auch häufiger an. Ich werde Rechner wohl unter den Schreibtisch verbannen, damit die Akustik nicht leidet.
* Die beiden Screens sind dasselbe Modell mit einer relativ hohen Auflösung. Wichtig eher für meine Adobe Creative Sessions als für Videokonferenzen.
* Keyboard und Mouse von Logitech aus der MX-Serie - endlich mal ein Keyboard, wo nicht alle wichtigen Tasten hinter Doppelbelegungen versteckt sind. Bluetooth Betrieb läuft wunderbar, auch ohne den USB Dongle, der hier ungenutzt auf dem Schreibtisch immer im Weg liegt.
* Router, Fritzbox, Printer, alles Standard und nicht besonders spannend.
* Elgato Stream Deck als programmierbares Zusatz-Keyboard. Elgato kommt aus der Gamer- und Video-Szene, das merkt man den Geräten an. Letztendlich solide Technik, ein bisschen überteuert aber gut einsetzbar.
* Elgato Key Light ist eine Videoleuchte, die einen eigenen WLAN Access hat und somit fernsteuerbar ist. Leider nur im 2G WLAN-Standard und leider nur über ein proprietäres Elgato-Protokoll. Die Einrichtung hakelt eine Weile, bis alles passt, aber dann läuft sie im Betrieb stabil. Lässt sich über das Stream Deck gut steuern. Die Farbtemperatur läßt sich einstellen. Für die Green Wall in 1,50 Metern Abstand ist eine Leuchte gerade noch ausreichend, in den schattigeren Bereichen flimmert der Bildschirm schon etwas. Besser wären zwei - aber mit über 200 EUR ist die Leuchte kein Schnäppchen...
* Die "Green Wall" ist bei mir eine leicht ausrollbare Standleinwand von celexon. Von Elgato gibt es auch eine etwas günstigere Variante, aber zum Einen ist die gerade nicht erhältlich - zu viel Nachfrage. Und außerdem stören mich dort die hervorstehenden Füße, die zu Stolperfallen werden können. 
* Die Haupt-Cam ist eine Logitech StreamCam, ein relativ neues Gerät mit ordentlicher Anzeigequalität und USB-C-Kabel. Ich habe kurz überlegt, ob ich meine Nikon DSLR einsetzen soll, habe mich aber dann dagegen entschieden. Noch mehr Technikkomponenten, die wieder gemanagt werden müssen - da ist für den Alltag die Webcam gut genug.
* Da ich mit meinen mobilen Geräten im Apple-Universum lebe, sind die zweite Cam und auch der Teleprompter auf diesen Geräten untergebracht. ein iPad dient als Anzeigemaschine für den Text, wenn es mal was Vorgefertigtes sein soll. Das iPhone steuert den Teleprompter (optional auch über ein Browser-Fenster. Ich hätte ja gerne eine REST-Schnittstelle, aber soweit sind die Client Apps meist noch nicht) oder dient als zweite Kamera. Und damit kommen wir auch zur im nächsten Bild dargestellten Struktur des Video Processing.

![OB und NDI](https://res.cloudinary.com/dzw4emsdt/image/upload/c_scale,w_900,q_auto/v1613339559/selfscrum/Portfolio-Digital_Consulting_ckt3dt.png)

Ich nutze zum einen NDI, das ist ein offener Videostreaming-Standard, der WLAN als Träger nutzt. Das klingt für ITler eher normal, in der Videobranche ist das ein Paradigmenwehcsel gewesen, da dort früher Video immer über proprietäre Leitungssysteme übertragen wurde.Für mich heißt das, dass ich mein iPhone oder iPad genauso als Eingangsquelle nutzen kann wie eine Webcam. NDI als Hersteller bietet noch viele weitere Utilities, zum Beispiel auch Screen Capturing.

Das wichtigste System ist zweifellos OBS, Open Broadcasting Studio, dass als Video-Effekt- und Mischsystem sehr gut geeignet ist, die verschiedenen Videoquellen zusammenzuführen und neu zu kombinieren. Praktischerweise gibt es ein NDI Plugin, so dass die WLAN-Image-Integration gesichert ist und auch ein Elgato Stream Deck Plugin sorgt für bessere Steuermöglichkeiten. So können bestimmte Konfigurationen auf Knopfdruck abgerufen werden.

Die eigentliche Funktion ergibt sich aus dem Zusammenstellen von verschiedenen Videoquellen zu Szenen. Ganz ähnlich wie bei Photoshop oder ähnlichen Programmen, ist jede Quelle eine Ebene, die individuell durch Positionierung, Skalierung und Filter manipuliert werden kann. Durch einen Filter wird zum Beispiel auch der Green Screen herausgefiltert, so dass dann die gefilmte Person mit anderem Hintergrund weiter genutzt werden kann. Die Ebenen ergeben übereinandergelegt eine Szene, die dann auf verschiedenen Ausgangswegen weitergeleitet wird, so zum Beispiel zur Videokonferenz. Praktischerweise können Szenen auch wieder als Datenquelle genutzt werden, so dass man sich eine kleine Szenenbibliothek anlegt, die dann immer wieder in anderen Zusammenhängen genutzt werden kann.

Das Thema Audio wird natürlich auch hier gemanagt. Dies ist allerdings noch einmal ein Kapitel für sich, so dass ich separat darauf eingehen werde.

Funktional bin ich mit dieser Kombination nun sehr zufrieden, weil sie alle Anforderungen abgeckt, die ich habe. Es gibt noch ein paar Integrations-Hakeleien zu bewältigen, so führt z.B. der vestärkte Einsatz des Stream Decks dazu, dass sich irgendwann die Webcam (oder der OBS-Treiber dafür) aufhängt. Aber das sind Kinderkrankheiten, die sich noch auswachsen werden. Die nächste Show kann beginnen!



