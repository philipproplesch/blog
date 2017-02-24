---
title: Autor in den Suchergebnissen von Google
date: 2012-03-15
tags: seo
---
Seit einiger Zeit verlinkt Google in den [SERPs](http://en.wikipedia.org/wiki/Search_engine_results_page) auf die [Google+](https://plus.google.com/)-Profile von Autoren. Dies geschieht aber keinesfalls willkürlich, sondern lässt sich mit wenigen Schritten auch für die eigene Seite bewerkstelligen.

![](/images/google_author_1.png)

Möchte man ebenfalls mit Bild in den Suchergebnissen auftauchen, sind zwei einfache Schritte nötig.

## Autor verlinken

Als erstes muss auf der gewünschten Seite bzw. dem Dokument ein Link zum Google+-Profil des Autors vorhanden sein. Wichtig hierbei ist der angehängte `?rel=author` Query-String-Parameter.

    <a href="https://plus.google.com/112441459154571338706?rel=author" rel="author" target="_blank">Google+</a>

## Autor “registrieren”

Anschließend muss die Root-URL der Zielseite im Google+-Profil des Autors eingetragen werden. Hierzu einfach in Google+ einloggen und in die Sektion "Über mich" wechseln. Danach unter Angabe eines Namens (z.B. Name der Seite) und der URL einen neuen Eintrag unter "**Macht mit bei**" hinzufügen.

![](/images/google_author_2.png)

## Validierung

Mit dem [Rich Snippets Testing Tool](http://www.google.com/webmasters/tools/richsnippets) lässt sich überprüfen ob die Implementierung geglückt ist.

![](/images/google_author_3.png)

Werden sowohl das Autoren-Bild, als auch die Meldung "**Verified: Authorship markup is verified for this page.**" angezeigt, dann heißt es warten. Warten auf den Googlebot, der diese Änderung hoffentlich übernimmt und in den SERPs darstellt.

![](/images/google_author_4.png)

Falls das Rich Snippets Testing Tool etwas zu beanstanden hat, liefert es in der Regel recht eindeutige und hilfreiche Fehlermeldungen zurück.
