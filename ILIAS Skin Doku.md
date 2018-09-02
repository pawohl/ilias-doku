# ILIAS Skin

ILIAS unterscheidet **System**- und **Content**-Styles.

* Content-Style: Beeinflusst ausschließlich die Darstellung von Inhalten; kann vollständig über die Benutzeroberfläche bearbeitet werden.
* System-Style: Header, Navigation, Footer kann teilweise über die Benutzeroberfläche bearbeitet werden.

Bei den System-Styles wird ferner nach **Style** und **Skin** unterschieden.

* Skin: Behälter für Styles und Sub-Styles, enthält unter anderem HTML-Vorlagen
* Style: Erlaubt eine erneute Auffächerung des Skins mit CSS/Less-Anpassungen

## Konfiguration

System- und Content-Styles können durch angemeldete ILIAS-Administratoren unter `Administration`→`Layout` bearbeitet werden.

## System-Styles

Eine Auflistung der installierten System-Styles befindet sich unter `Administration`→`Layout`. Dort können neue Skins/Styles angelegt werden, vorhandene bearbeitet und gelöscht werden. Es ist auch möglich Benutzern eines Skins/Styles einen anderen zuzuweisen. Einzelbenutzern kann unter `Administration`→`Benutzerverwaltung` ein anderer Skin/Style zugewiesen werden.
Dazu klickt man auf einen Benutzernamen, dann wählt man im Abschnitt `Einstellungen` den gewünschten `Standard-Skin/-Style`.

Inkompatible Änderungen an System-Styles können dazu führen, dass die Benutzeroberfläche nicht mehr aufgerufen werden kann. Der in Entwicklung befindliche Skin/Style könnte einem Testnutzeraccount zugewiesen werden.

### Bearbeiten eines System-Styles

Änderungen an einigen Größen- und Farbangaben können über die Benutzeroberfläche vorgenommen werden. Dazu wählt man die Aktion `Bearbeiten` eines Style/Skin aus.

Im Bereich `Less` und `Icons` stehen ein Funktionen zum Bearbeiten zur Verfügung.

> **Less** (Leaner Style Sheets; sometimes stylized as LESS) is a dynamic preprocessor style sheet language that can be compiled into Cascading Style Sheets (CSS)

Less erlaubt neben der Definition von Regeln auch die Definition von **Variablen**. Dies wird extensiv für Farben und Größenangaben von ILIAS genutzt.

ILIAS baut in Sachen Style auf dem Front-end framework *Bootstrap* auf.

Weiterführende Informationen bietet [die ILIAS Skin-Designer Doku](https://www.ilias.de/docu/goto_docu_pg_68693_367.html) und die [Entwickler-Doku](https://www.ilias.de/docu/goto_docu_pg_38739_42.html).

#### Beispiel
Soll das Logo vergrößert werden, so muss auch der Header größer werden, und der Inhalt weiter nach unten rutschen. Das muss dank less nicht von Hand erledigt werden.

An einer Stelle wird die Standardgröße des Logos definiert:

```less
@il-logo-height: 50px;
```

Dieser Wert kann in der Benutzeroberfläche überschrieben werden. Die Definition der Header-Höhe ergibt sich aus der Logo-Höhe plus einer Konstanten:

```less
@il-main-header-height: 80px + @il-logo-height;
```

Benutzt wird der Wert `il-main-header-height` dann in einer anderen Datei, um dem Header seine Höhe zuzuweisen:

```less
/* ... */
.ilMainHeader {
	background-color: @il-main-header-bg;
	height: @il-main-header-height;
	border-bottom: @il-header-border-bottom-height solid @il-header-border-bottom-color;
	/* ... */
}
```

Führt man über die Benutzeroberfläche in ILIAS eine Änderung an den Variablen durch und speichert diese mit `Variablen aktualisieren` ab, schreibt ILIAS diese in die Datei `[stylename]-variables.less`, z.B. `startup-tutor-variables.less` im Skin-Verzeichnis. Anschließend ruft ILIAS den **Less-Compiler** `lessc` auf.
Dieser ist in Ihrer ILIAS-Docker-Variante **bereits installiert**.

Der Less-Compiler wandelt alle Less-Dateien in eine große CSS-Datei namens `[stylename].css` um.
Diese CSS-Datei kann schnell durch den Browser Ihrer Besucher geladen werden, während die `less`-Dateien Ihnen erlauben, Ihren Skin/Style flexibel anzupassen ohne redundante Änderungen vornehmen zu müssen.

#### Workflows

Sie wissen nun, wie das System arbeitet. Hier einige Vorschläge zum Vorgehen.

##### Anpassen von Less-Variablen (einige Farben, einige Größenangaben)

Das geht bequem über die Benutzeroberfläche.

##### Darüber hinaus gehende Änderung des Stylesheets

Im Basis-Verzeichnis (s. Zugriff auf das Basis-Skin-Verzeichnis) Ihres Skins befindet sich die Datei `[stylename].less`. In diese können Sie beliebige CSS-Deklarationen schreiben oder weitere `less`-Stylesheets einbinden:

```less
// Vorlagen-Datei wird als Standard (Delos) im
// ersten Schritt importiert
@import "../../../../templates/default/delos";

// Anschließend überschreiben die in der
// Benutzeroberfläche gesetzten Variablen die
// Standardvariablen
@import "startup-tutor-variables.less";

// Weil Sie sehr viele Schriftarten-Definitionen
// hinzufügen, haben Sie sich entschieden, diese
// in eine Extra-Datei, die im Verzeichnis "less",
// zu finden innerhalb Ihres Skin-Verzeichnisses,
// zu schreiben.
// Diese Datei importieren Sie an dieser Stelle,
// damit die Regeln nach der Kompilierung mit
// `lessc` in der [skinname].css verfügbar sind.
@import "less/font.less";

// CSS-Definition: Den Selektor haben Sie so
// gewählt, dass die Regeln die Delos-Regeln
// überschreiben
#ilTopNav .navbar-nav > li > a {
	@media only screen and (max-width: @grid-float-breakpoint-max) {
		color: #a1c73d;
		font-weight: bold;
	}
}

// Eine CSS-Definition, deren Selektor es im
// Standard nicht gibt, um den Instanz-Titel zu
// verstecken
.ilTopTitle {
	visibility: hidden;
}

// ...
```

Nach Änderungen an `less`-Dateien muss die CSS-Datei **neu erzeugt** werden. Am einfachsten geht das, indem Sie auf `Variablen aktualisieren` in der Web-Benutzeroberfläche unter `Administration`→`Layout`→`[Skin]`→`Aktionen:Bearbeiten` anklicken.
Sollten Ihre `less`-Änderungen ungültig sein, erhalten Sie eine Fehlermeldung.

Um die Variablennutzung begutachten zu können, ist es hilfreich, die verwendeten `less`-Dateien zu kennen.
So finden Sie **alle `less`-Dateien** in der aktuellen ILIAS-Version:

* Im ILIAS-Quellcode-Verzeichnis: `find . -path ./data -prune -o -name "*.less"`
* [Auf GitHub](https://github.com/ILIAS-eLearning/ILIAS/tree/release_5-3): Wählen Sie den korrekten Entwicklungszweig (Branch) und suchen Sie: `extension:less`
* Die meistrelevanten Dateien dürften in der [`templates/default/delos.less`](https://github.com/ILIAS-eLearning/ILIAS/blob/release_5-3/templates/default/delos.less) verlinkt sein. Bitte den richtigen Entwicklungszweig (Branch) wählen.

So **durchsuchen** Sie nur **Less-Dateien**:

* Im ILIAS-Quellcode-Verzeichnis: `grep -r --include \*.less "Suchmuster"`
* [Auf GitHub](https://github.com/ILIAS-eLearning/ILIAS/tree/release_5-3): Wählen Sie den korrekten Entwicklungszweig (Branch) und suchen Sie: `Suchmuster extension:less`

##### Änderungen an HMTL-Vorlagen
Nicht alle Änderungen lassen sich allen mit Less oder CSS ausführen. Soll beispielsweise ein weiterer Bereich mit Logo zum Footer hinzugefügt werden, ist das nur schwer mit Stylesheets umzusetzen.
ILIAS arbeitet mit Vorlagen (*Templates*). Diese sind relativ willkürlich nur für bestimmte Bereiche (Footer) oder Seiten (Inhaltsseiten, Admin-Seiten, Fehlerseiten, Anmeldeseiten) gültig.

Anpassungen können Sie vornehmen, indem Sie HTML-Vorlagen an bestimmte Orte in Ihrem Skin-Verzeichnis legen. Ihre HTML-Vorlagen überschreiben dann die Standard ILIAS Vorlagen.
Der Ablageort, an welchem die Vorlage im ILIAS-Quellcode-Verzeichnis gespeichert ist, bestimmt den Ablageort in Ihrem Skin-Verzeichnis.

Um Vorlagen anzupassen, muss man diese finden. So **durchsuchen** Sie nur **HTML-Dateien**:

* Im ILIAS-Quellcode-Verzeichnis: `grep -r --include \*.html "Suchmuster"`
* [Auf GitHub](https://github.com/ILIAS-eLearning/ILIAS/tree/release_5-3): Wählen Sie den korrekten Entwicklungszweig (Branch) und suchen Sie: `Suchmuster extension:html`

**Beispiel**: Sie möchten, dass der Link zum Impressum im Footer vor der Kopiervorlage für den Link zur aktuellen Seite angezeigt wird. Um die Vorlage zu finden, haben Sie den HTML-Quelltext der Ausgabe inspiziert (Developer Tools Ihres Browsers). Sie haben gesehen, dass es ein Element mit der CSS-Klasse `ilFooterContainer` gibt. Sie suchen danach und sehen, dass es 3 Dateien gibt, die diese Klassen enthalten:

```
templates/default/tpl.error.html
Services/Init/templates/default/tpl.startup_screen.html
templates/default/tpl.adm_content.html
```

Würden Sie alle 3 Dateien anpassen wollen, würden Sie diese wie folgt in Ihrem Skin-Verzeichnis ablegen:

```
tpl.error.html          -> Wurzel-Verzeichnis Ihres Skins
tpl.startup_screen.html -> Services/Init/tpl.startup_screen.html
tpl.adm_content.html    -> Wurzel-Verzeichnis Ihres Skins
```

Heinweis: Die Fehlerseite wird Benutzern nicht standardmäßig angezeigt. Es ist wahrscheinlich nicht notwendig, diese anzupassen. Wenn eine Anpassung vorgenommen wird, muss unbedingt geprüft werden, ob diese funktioniert. ILIAS ist sehr *unforgiving* was Fehler in Vorlagen angeht.

Prinzipiell könnte man alle Vorlagen aus dem ILIAS-Quellcode-Verzeichnis kopieren. Dann würde man aber die Anpassungen nicht mehr gut erkennen. Daher empfehle ich, wirklich nur die Dateien zu kopieren, die angepasst werden müssen.

## Zugriff auf das Basis-Skin-Verzeichnis

Auf Ihr Skin-Verzeichnis können Sie wahlweise SFTP oder SSH-Zugang innerhalb Ihres Unternehmensnetzes erhalten.

Hinweis: Nach Änderungen an `less`-Dateien müssen diese neu kompiliert werden, falls eine Änderung im Browser sichtbar werden soll. Am einfachsten geht das, indem Sie auf `Variablen aktualisieren` in der Web-Benutzeroberfläche unter `Administration`→`Layout`→`[Skin]`→`Aktionen:Bearbeiten` anklicken. Sollten Ihre `less`-Änderungen ungültig sein, erhalten Sie eine Fehlermeldung.

Manchmal werden die Änderungen selbst nach erfolgreicher Kompilierung nicht sofort im Browser angezeigt. Das liegt am **Browser Cache**. In diesem Fall halten Sie die Umschalttaste <kbd>⇧</kbd> gedrückt, während Sie auf den Knopf "Neu laden" Ihres Browsers drücken. Oder Sie schließen alle anonymen/inkognito/in private Fenster und öffnen die URL in einem Neuen privaten Fenster.

### SFTP-Zugriff

Um per SFTP zuzugreifen benötigen Sie einen kompatiblen Client. [FileZilla](https://filezilla-project.org/) (open source, kostenlos) ist so einer.

Auf der einen Seite zeigt FileZilla ein lokales Verzeichnis und sobald eine Verbindung zum Server hergestellt ist, auf der anderen Seite das Server-Verzeichnis. Sie können Datein per Drag&Drop von der einen auf die andere ziehen.

Um auf Ihr Skin-Verzeichnis zuzugreifen klicken Sie auf `Datei`>`Servermanager ...`. Der Servermanager erlaubt Ihnen, die Verbindungsdetails zu Ihrem SFTP-Server so abzuspeichern, dass Sie später leichter wieder darauf zugreifen können.

Klicken Sie auf `Neuer Server`. Vergeben Sie einen Namen. Wählen Sie den Namen so, dass Sie ihn wieder erkennen, z.B. `startup tutor skin`.

Tragen Sie folgende Details ein:

* Server: Hostname des Servers: Domain Name oder eine IP-Adresse, z.B. `tutor.gruendung.uni-halle.de` (kein Protokoll oder Schema wie `https://` oder `sftp://`)
* Port: Die Portnummer habe ich Ihnen mitgeteilt. Meist `22` oder `2222`.
* Protokoll: `SFTP`
* Verbindungsart: Habe ich Ihnen mitgeteilt.
* Die weiteren Punkte habe ich Ihnen ebenfalls mitgeteilt. Falls die Authentifizierung mit Passwort erfolgt, haben Sie die Wahl, ob Sie das Passwort auf Ihrer Festplatte speichern lassen möchten (`Normal`) oder bei jeder Verbindung eintippen möchten. Wenn Sie viel mit Ihrem Gerät reisen oder unsichere Software darauf ausführen, sollten Sie das Passwort nicht im Gerät speichern.

### SSH-Zugriff
Auch hier haben Sie die Wahl, ob Sie lieber ein Terminal benutzen oder einen File-Manager oder eine Kombination.
[mobaxterm](https://mobaxterm.mobatek.net/) (kostenlose Version verfügbar) erlaubt Ihnen z.B. beides zugleich.

Vorteil von SSH: Eine (Git-)Versionierung Ihres Skins ist möglich.
