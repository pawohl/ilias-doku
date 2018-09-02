# ILIAS Startseite

Die Startseite, welche Benutzende bei Aufruf von https://domain.example.com/ angezeigt bekommen, muss über die Web-Server-Konfiguration festgelegt werden. Eine Möglichkeit über die ILIAS-Startseite ist mir nicht bekannt.

Ich unterstütze Sie gern bei der Umsetzung Ihrer Startseite.

Folgende Optionen für eine Startseite stehen prinzipiell zur Verfügung:

* Login-Seite
* Startseite des öffentlichen Magazin-Bereichs oder ein öffentliches Lernmodul/ eine öffentliche Ressource
* Eine selbst gestaltete HTML-Seite. Diese hätte einige Einschränkungen, würde aber ggf. wesentlich schneller laden, als eine ILIAS-Seite.
* Impressum/ Nutzungsbedingungen

Voraussetzung für das Hinzufügen von Rich-Text-Inhalten ist, dass unter `Administration`→`Editor`→`ILIAS-Seiteneditor`→`Magazinseiten` die Option *Aktiviere Seitenbearbeitung im Magazin* aktiviert ist.

## Persönliche Startseite

Neben der allgemeinen Startseite kann eine persönliche Startseite für Benutzer festgelegt werden. Unter `Administration`→`Benutzerverwaltung`→`Einstellungen`→`Startseiten` besteht die Auswahl, ob die persönliche Startseite die Übersicht, Kurse und Gruppen, der Arbeitsraum, der Kalender, das Magazin oder ein Magazin-Objekt sein soll.

## Gestaltung der Login-Seite

Unter `Administration`→`Authentifizierung / Neuanmeldung`→`Authentifizierung`→`Loginseite gestalten` können Rich-Text-Inhalte zur Login-Seite hinzugefügt werden.

Dieser Text sollte sich über `Administration`→`Layout`→`Content Style`→`Name des aktiven Content Styles` stilistisch verändern lassen. Damit und mit CSS/Less könnte ggf. schon die gewünschte Anpassung erzielt werden, ohne die HTML-Templates bearbeiten zu müssen.

Folgende HTML-Vorlagen sind relevant:
1) `Services/Init/tpl.login.html`
   Enthält die Details und ist spezifisch für das Login-Formular.
2) `Services/Init/tpl.startup_screen.html`
   Ist der äußere Rahmen, welcher unter anderem fürs Login-Formular, aber beispielsweise auch für die Nutzungsvereinbarung verwendet wird.

Wichtig: Die HTML-Kommentare (`<!-- -->`) müssen in der Datei stehen bleiben und weiterhin die gleichen Elemente einschließen. Bestimmte Elemente werden von der ILIAS PHP-Anwendung durch die Kommentare gesucht, vervielfältigt, geändert oder entfernt. Daher sind die HTML-Kommentare unerlässlich. Sind sie fehlerhaft, könnte ILIAS vollständig den Dienst verweigern.

Weitere Änderungen an Login-Seiten deren Vorlagen noch nicht im Skin-Verzeichnis liegen, lassen sich aus dem [ILIAS GitHub repository](https://github.com/ILIAS-eLearning/ILIAS/tree/release_5-3/Services/Init/templates/default) kopieren und unter `Services/Init/` im Skin-Verzeichnis mit gleichem Namen einfügen.

## Gestaltung des Magazins

Unter `Magazin`→`Magazin - Einstiegsseite` können im Reiter `Inhalt`→`Seite gestalten` Rich-Text-Inhalte hinzugefügt werden. Die Präsentation des Magazins kann im Reiter `Inhalt` und `Einstellungen` vorgenommen werden.
