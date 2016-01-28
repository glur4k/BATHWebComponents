# Web Components nach dem vorläufigen W3C Standard

In diesem Kapitel wird auf die Problemlösungen der Web Components nach den Vorstellungen des W3C eingegangen. In Abschnitt 2.2 wird die erste Technologie vorgestellt, die Custom Elements, in Abschnitt 2.3 wird auf den Shadow DOM eingegangen, Abschnitt 2.4 widmet sich den HTML Templates und Abschnitt 2.5 zeigt die letzte Technologie, die HTML Imports. In Abschnitt 2.6 werden die Polyfills erklärt, welche für die Technologien noch zwingend notwendig sind.


## Problemlösung

In der heutigen Webentwicklung kommt es häufig vor, dass oftmals für diverse Probleme die gleiche, oder eine ähnliche Lösung programmiert werden muss. So muss auf vielen Seiten ein Slider, eine Navigation oder eine andere Komponente, welche das gewünschte Feature beinhaltet, eingebunden werden. Diese unterscheiden sich stets leicht, bringen im Kern aber dennoch meist die selben Funktionen mit sich. Um diese Funktionen auf der Webseite verfügbar zu machen sind eine Reihe an verschiedenen Technologien notwendig. Wenn die gewünschte Komponente bereits existiert, muss für das Einbinden dieser Komponente selbst, ein bestimmtes HTML Markup geschrieben werden. Damit die Komponente nun funktioniert, muss ein JavaScript eingebunden werden, welches zusätzlich noch anhand einer vordefinierten API konfiguriert werden muss. Diese API ist in der Regel nur für diese eine Komponente entworfen, so müssen für jede Komponente unterschiedliche APIs angesprochen werden, die sich mit unter stark unterscheiden können. Damit die Komponente dann auch in das optische Äußere, in das Design der eigenen Webseite passt, muss ebenso ein entsprechendes Stylesheet mit den Style-Definitionen eingebunden werden. Meist bringt das jedoch auch Styles mit, die sich negativ auf die Seite auswirken, da sie nicht sauber gekapselt sind. In diesem Fall muss auch das Stylesheet noch nachgebessert werden. Nimmt man diese Punkte zusammen, so wird deutlich, dass es in der Webentwicklung kein Plugin-System gibt, mit dem Webseiten schnell und einfach erweitert werden können.
Diesem Problem widmen sich die Web Components. Sie sollen der Frontend-Entwicklung ein Plugin-System bereitstellen, welches es ermöglicht, fremde und eigene Komponenten schnell und einheitlich in die eigene Seite einzubinden. Eine Komponente steht dabei als eigenes HTML Element, welches ihre gesamte innere Funktionalität in sich kapselt und nach Außen unsichtbar macht. Konflikte mit anderen Komponenten oder der einbindenden Webseite selbst werden somit vermieden. Dabei ist das Verhalten nach außen für jede Komponente das selbe, es gibt also für jede Komponente die gleiche Schnittstelle um sie zu konfigurieren und einzubinden. Dies erleichtert den Umgang mit Plugins deutlich, da die einzige dafür notwendige Technologie HTML selbst ist. Dadurch können einzelne Komponenten verwendet werden wie jedes HTML Element. Sie sind verschachtelbar haben Attribute, über welche sie konfiguriert werden können. Web Components bilden dabei eine Sammlung an Technologien, um jene Eigenschaften zu gewährleisten. Im folgenden Kapitel werden die grundlegenden Technologien erklärt und auf ihre Anwendung eingegangen.