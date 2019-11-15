mpMinify 0.1 Beta für CONTENIDO 4.8.x

####################################################################################################
TOC (Table of contents)

- BESCHREIBUNG
- CHANGELOG
- BEKANNTE PROBLEME
- FEATURES
- VORAUSSETZUNGEN
- INSTALLATION
- WICHTIGES ZUM INHALT
- TIPPS
- SCHLUSSBEMERKUNG



####################################################################################################
BESCHREIBUNG

minify ist eine PHP Applikation, die mehrere JavaScript- oder CSS-Dateien kombinieren,
komprimieren und mit einem entsprechenden Header ausgeben kann, so dass die Anzahl
der Anfragen (Requests) auf eine Webseite und die versendete Datenmenge reduziert wird.

Mit mpMinify für CONTENIDO kann man die Ausgabe der JavaScript- oder CSS-Dateien
im Frontend einer CONTENIDO-Installation optimieren.

Beispiel:
Auf der Webseite werden z. B. 3 JavaScript- und 5 CSS-Dateien eingebunden.
Wenn man die Webseite besucht, macht der Browser 8 Anfragen, um die Dateien
zu laden und der Server liefert die 8 Dateien aus.

Mit minify kann man es im Idealfall erreichen, dass der Browser nur 2 Anfragen
an den Server sendet, jeweils eine für JavaScript und für CSS-Datei.

Bei entsprechender Konfiguration kümmert sich minify darum, die Dateien zusammenzuführen,
und zu komprimieren, so dass der Browser wenig Requests hat und der Server weniger
Daten versendet.

Anderes Beispiel:
Auch wenn man verschiedene JavaScript- und 5 CSS-Dateien Dateien nicht zusammenführen
möchte, ist die Verwendung von minify von Vorteil. Jede einzelne über minify
verarbeitete Datei wird von unnötigem Inhalt (Leerzeichen und Kommentare) entfernt
und komprimiert, so dass am Ende die Menge der vom Server an den Browser versendeten
Daten reduziert wird.

Das Reduzieren der Anzahl der Requests und das Reduzieren der Datenmenge ist sowohl
für den Browser als auch für den Server eine übliche Optimierungsmaßnahme.



####################################################################################################
BEKANNTE PROBLEME

Momentan keine



####################################################################################################
CHANGELOG

2012-10-26: mpMinify 0.1 Beta (for CONTENIDO 4.8.x)
    * First beta release



####################################################################################################
FEATURES

- Optimierte Ausgabe von JavaScript- oder CSS-Dateien im Frontend



####################################################################################################
VORAUSSETZUNGEN

- Alle Voraussetzungen von CONTENIDO 4.8.x gelten auch für mpMinify



####################################################################################################
INSTALLATION

Beispielhaft wird im Folgenden davon ausgegangen, dass das Mandantenverzeichnis 'cms' lautet.

Dateien aus dem mpMinify-Package in die entsprechenden CONTENIDO-Verzeichnisse kopieren.
- 'vendor' in das CONTENIDO Installationsverzeichnis kopieren
- 'cms' oder den Inhalt davon in das Mandantenverzeichnis kopieren

Die Datei 'cms/min/index.php' öffnen und für den Mandanten anpassen
- Werte in $min_con_config überschreiben Einstellungen von 'vendor/minify/min/config.php'
- Werte in $min_con_config_test überschreiben Einstellungen von 'vendor/minify/min/config-test.php'
- Werte in min_con_groupsConfig überschreiben Einstellungen von 'vendor/minify/min/groupsConfig'

Anpassen der URLs zu JavaScript- oder CSS-Dateien im Layout


Anwendungsbeispiel:
Im Layout werden folgende Dateien eingebunden
[code]
<script src="js/awesomelib.js" type="text/javascript"></script>
<script src="js/awesomelib-tooltip.js" type="text/javascript"></script>
<script src="js/awesomelib-gallery.js" type="text/javascript"></script>
<link rel="stylesheet" href="css/main.css" type="text/css" media="all" />
<link rel="stylesheet" href="css/navigation.css" type="text/css" media="all" />
<link rel="stylesheet" href="css/forms.css" type="text/css" media="all" />
<link rel="stylesheet" href="css/tooltip.css" type="text/css" media="all" />
<link rel="stylesheet" href="css/gallery.css" type="text/css" media="all" />
[/code]

Um das mit minify zu optimieren, kann man folgende Schritte machen:

In der Datei 'cms/min/index.php' die Konfiguration für Gruppen setzen.
[code]
$min_con_groupsConfig = array(
    'js' => array(
        '//cms/js/awesomelib.js',
        '//cms/js/awesomelib-tooltip.js',
        '//cms/js/awesomelib-gallery.js'
    ),
    'css' => array(
        '//cms/css/main.css',
        '//cms/css/navigation.css'
        '//cms/css/forms.css'
        '//cms/css/tooltip.css'
        '//cms/css/gallery.css'
    ),
);
[/code]
Der Prefix '//cms/' ist für minify und referenziert den Pfad der Datei vom 
Documentroot des Webservers aus.

Beispiel:
/var/www/ -> Document Root
/var/www/cms -> Mandantenverzeichnis
/var/www/cms/js/awesomelib.js -> JavaScript Datei im Mandantenverzeichnis
minify Wert -> //cms/js/awesomelib.js

Im Layout die Angaben ändern in
[code]
<script src="min/?g=js" type="text/javascript"></script>
<link rel="stylesheet" href="min/?g=css" type="text/css" media="all" />
[/code]
Mit der URL "min/?g=js" werden alle Dateien in der Gruppe "js" zusammengefügt
und komprimiert, also die 3 JavaScript-Dateien.

Mit der URL "min/?g=css" werden alle Dateien in der Gruppe "css" zusammengefügt
und komprimiert, also die 5 CSS-Dateien.

Der erste Aufruf im Frontend kann etwas länger als üblich dauern, da minify die
Dateien verarbeitet und cached. Danach geht es viel schneller.



####################################################################################################
WICHTIGES ZUM INHALT

cms/min/index.php:
------------------
Die minify Konfiguration für das Frontend des Mandanten.


vendor/minify:
--------------
Die Sourcen der minify Applikation.
vendor/minify/README.txt -> Sollte man sich immer durchlesen
vendor/minify/MIN.txt -> Enthält weitere Anwendungsbeispiele zu minify



####################################################################################################
TIPPS

Bei Verwendung einer mod_rewrite Lösung kann es nötig sein, Anfragen auf das 'min'
Verzeichnis im Mandantenordner vom Umschreiben ausschließt.



####################################################################################################
SCHLUSSBEMERKUNG

Benutzung auf eigene Gefahr!

Murat Purc, murat@purc.de
