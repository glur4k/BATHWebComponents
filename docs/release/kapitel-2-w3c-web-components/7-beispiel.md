## Implementierung einer Komponente mit den nativen Web Component APIs

- TODO:
    - Abbildung X
    - "TODO Verweis" 2x

Anhand der vorhergehenden Abschnitte wird in diesem Abschnitt die Implementierung der Web Komponente `<custom-element>` mit den nativen HTML APIs erläutert. Diese soll dabei das Markup in einem Shadow DOM kapseln und den übergebenen Inhalt darstellen. Des Weiteren soll dessen Farbe über das Attribut `theme` optional konfiguriert werden können. Die gerenderte Komponente wird in Abbildung X dargestellt.

![Bild: Gerenderte Web Komponente mit nativen APIs](images/7-beispiel.jpg "Gerenderte Web Komponente mit nativen APIs")


### Custom Element erstellen und Eigenschaften und Funktionen definieren

- Zunächst wird ein `HTMLElement` Prototyp mittels `Object.create(HTMLElement.prototype)` erstellt
- Dieser wird zunächst um die Eigenschaft `theme` und dessen Standardwert `style1` erweitert `CustomElementProto.theme = 'style1';`, welches das deklarativ konfigurierbare Attribut `theme` darstellt
- Anschließend werden Lifecycle-Callback-Funktionen `createdCallback` und `attributeChangedCallback` der Komponente definiert

```javascript
  CustomElementProto.createdCallback = function() {
    if (this.hasAttribute('theme')) {
      var theme = this.getAttribute('theme');
      this.setTheme(theme);
    } else {
      this.setTheme(this.theme);
    }
  };

  CustomElementProto.attributeChangedCallback = function(attr, oldVal, newVal) {
    if (attr === 'theme') {
      this.setTheme(newVal);
    }
  };
```

- Die `createdCallback` Funktion soll zunächst prüfen ob das Attribut `theme` beim Verwenden des `<custom-element>` Tags verwendet und ein entsprechender Wert gesetzt wurde und übergibt dieses der Hilfsfunktion `setTheme`
- Wird das Attribut nicht gesetzt, wird der Standardwert `style1` übergeben
- Falls das `style` Attribut von Außen geändert wird, soll die `attributeChangedCallback` Funktion gewährleisten, dass die Änderung auch von der Komponente übernommen wird, indem sie das Attribut der Hilfsfunktion `setTheme` übergibt
- Um das Setzen und Ändern des `theme` Attributs zu implementieren wird zuletzt die Hilfsfunktion `setTheme` für den Prototyp definiert

```javascript
  CustomElementProto.setTheme = function(val) {
    this.theme = val;
    this.outer.className = "outer " + this.theme;
  };
```

- Sie setzt den übergebenen Parameter, das `theme` Attribut, als Klasse auf den umschließenden Wrapper `.outer`
- Die gesetzte Klasse bestimmt dabei den zu verwendenden Style der Komponente
- Da der Prototyp nun alle erforderlichen Eigenschaften besitzt, kann er mit `document.registerElement("custom-element", { prototype: CustomElementProto });` als HTML Tag `custom-element` in dem importierenden Dokument verfügbar gemacht werden


### Template erstellen und Styles definieren

- Das Element benötigt außerdem noch enthaltenes HTML Markup, also ein Template
- Hierfür wird ein `<template>` mit der ID `myElementTemplate` angelegt, welches die für die Komponente notwendige HTML Struktur beinhaltet

```html
<template id="myElementTemplate">
  <div class="outer">
    Welcome in the Web Component
    <div class="name">
      <content></content>
    </div>
  </div>
</template>
```

- So wird der von Außen zu definierte Inhalt mittels dem `<content>` Tag in die Komponente übergeben und dargestellt
- Zusätzlich werden zwei Hilfs-Wrapper und Text definiert, damit die Elemente schneller mittels JavaScript selektierbar sind
- Um nun zwischen mehreren Styles, welches mittels dem `theme` Attribut ausgewählt werden kann, zur Verfügung zu stellen, werden diese in einem `<style>` Tag in dem Template definiert
- In diesem Beispiel werden zwei Optionen, `style1` und `style2`, sowie weitere Styles für die gesamte Komponente definiert

```html
<style>
  .outer { /* Some minor styles */ }
  .style1 { color: green; }
  .style2 { color: blue; }
  .name { font-size: 35pt; padding-top: 0.5em; }
</style>
```

- Das Template wird nun zwar schon heruntergeladen, jedoch noch nicht in den DOM eingefügt
- Hierzu muss es dem Shadow Root hinzugefügt werden, was in dem nachfolgenden Abschnitt veranschaulicht wird


### Template bereitstellen und Shadow DOM zur Kapselung benutzen

- Bevor das erstellte Template eingebunden werden kann, muss zuerst ein Shadow Root mittels `var shadow = this.createShadowRoot();` erzeugt werden
- Dies geschieht in der `createdCallback` Funktion, damit der Shadow DOM sofort erzeugt werden kann, wenn das Element erzeugt wurde
- Nun kann der Inhalt des Templates mit der ID `myElementTemplate` mittels `var template = importDoc.querySelector('#myElementTemplate').content;` importiert und mit der Anweisung `shadow.appendChild(template.cloneNode(true));` dem Shadow Root hinzugefügt werden
- die `importDoc` Variable stellt dabei die Referenz auf die importierte Komponente, also das `<custom-element>` Element, dar
- Diese kann mittels der Funktion `var importDoc = document.currentScript.ownerDocument;` ermittelt werden
- Wird dies nicht getan, so würde der `querySelector` auf das Eltern Dokument der eingebetteten Komponente zugreifen und das Template nicht finden
- Nun ist der Inhalt des Templates als Shadow DOM innerhalb des Elements gekapselt und nach Außen nicht sichtbar


### Element importieren und verwenden

- Das Element ist somit vollständig und kann in einer beliebigen Webseite oder Applikation eingesetzt werden
- Hierzu muss diese das Element mittels `<link rel="import" href="elements/custom-element.html">` zunächst importieren
- Es kann dann mit entsprechenden Attributen und Inhalt auf der Seite eingebettet werden, wie beispielsweise mit der Konfiguration `<custom-element theme="style1">Reader</custom-element>`
- Das komplette Beispiel der Komponente (TODO Verweis), sowie dessen Einbindung in ein HTML Dokument (TODO Verweis), sind im Anhang zu finden.
