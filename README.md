# Sungrow_iHomeManager_to_iDM_Heatpump

## Anbindung einer iDM-Wärmepumpe an den Sungrow iHomeManager über Node-RED

Der iDM Navigator ist momentan (Stand: 05/2026) nicht in der Lage, Daten von einer Photovoltaik-Anlage zu beziehen, die den Sungrow iHomeManager nutzt. Da iDM mir signalisiert hat, an einer Unterstützung des iHomeManager kein Interesse zu haben, habe ich meine eigene Anbindung entwickelt.
Nach einer Evaluation verschiedener Optionen fiel meine Wahl auf Node-RED. Node-RED war zu diesem Zeitpunkt neu für mich. Wer mit Node-RED mehr Erfahrung hat, wird möglicherweise einfachere Möglichkeiten der Umsetzung finden. Für mich stand zu diesem Zeitpunkt im Vordergrund, daß die Umsetzung einfach und überschaubar ist, und vor allem: daß sie funktioniert.

## Hardware

Als Hardware benötigt man irgendeinen Computer, auf dem Node-RED läuft. Informationen dazu findet man auf der [Node-RED-Webseite](https://nodered.org/docs/getting-started/).

Dieser Computer sollte sich im selben Netzwerk befinden, in dem auch der iHomeManager und der iDM Navigator erreichbar sind.

Ich habe mich für einen Raspberry Pi 3B+ entschieden. Als Betriebssystem verwende ich [DietPi](https://dietpi.com). Vermutlich genügt auch ein älteres Raspberry-Pi-Modell, da die zu leistende Arbeit nicht besonders anspruchsvoll ist. An Erfahrungsberichten wäre ich sehr interessiert. Vor allem fände ich spannend, ob es mit einem Raspberry Pi Zero funktioniert.

## Einrichtung

Auf dem iHomeManager sollte Port 503 (ohne SSL) freigeschaltet sein. Es funktioniert auch mit einem der Ports 502 oder 504, dann muß die Konfiguation in Node-RED aber etwas angepaßt werden.

Im iDM Navigator wählt man unter "Einstellungen" - "Photovoltaik" - "PV Signal" die Option "Gebäudeleittechnik / Smartfox".

Sobald das Betriebssystem und Node-RED installiert sind, kann man sich über einen Webbrowser mit Node-RED verbinden. Node-RED ist unter der IP-Adresse des Rechners, auf dem die Installation läuft, über den Port 1880 erreichbar.

Im Burger-Menü findet man "Palette verwalten" und dort "Installation. Folgende Module werden benötigt:

* node-red-contrib-buffer-parser
* node-red-contrib-modbus

Unter "Import" kann man dann die Datei ```node_red_flow.json``` hochladen oder ihren Inhalt über die Zwischenablage einfügen.

Zuletzt ruft man noch die Konfigurations-Seite über das Zahnrad (rechts, unterhalb des Burger-Menüs) auf. Dort müssen ide Konfigurations-Nodes _iHomeManager_ und _iDM_ angepaßt werden. In beiden Nodes muß die jeweilige IP-Adresse eingetragen werden. Falls man den iHomeManager über einen anderen Port als 503 ansprechen möchte, wird das im Konfigurations-Node des _iHomeManager_ gemacht.

Sind alle Einstellungen fertig, so kann man oben auf den Knopf "Übernehmen (deploy)" klicken und dadurch die Änderungen aktivieren. Ab diesem Moment müßte die Datenübertragung laufen.

## Daten überprüfen

Im iDM Navigator kann man nun unter "Einstellungen" - "Photovoltaik" - "PV Leistung" nachsehen, ob die Daten bei der Wärmepumpe ankommen. Die Werte werden alle 5 Sekunden aktualisiert.
