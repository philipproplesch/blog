---
title: Cannot read configuration file because it exceeds the maximum file size
date: 2012-04-02
tags:
  - asp.net
  - iis
---
![](/images/webconfig_size_exceeded_1.png)

Standardmäßig beschränkt der [IIS](http://www.iis.net/) die maximale Dateigröße für *.config-Dateien auf 250KB. Wird dieses Limit überschritten, erscheint die abgebildete Fehlermeldung.

In einem aktuellen Projekt sollten viele (sehr viele!) alte URLs *gerettet* werden und mittels [URL Rewrite](http://www.iis.net/downloads/microsoft/url-rewrite) auf die neuen, korrespondierenden Zielseiten weitergeleitet werden. Aufgrund der immensen Anzahl an Rewrites wurden die 250KB relativ schnell erreicht und auch die Auslagerung in eine eigene Datei via *configSource*-Attribut hat hier nicht helfen können.

Die Lösung des Problems war ein einfacher Eintrag in der Registry, der unter *HKLM\SOFTWARE\Microsoft\InetStp\Configuration* angelegt werden musste. Klingt dreckig, funktioniert aber ;)

![](/images/webconfig_size_exceeded_2.png)

Weitere Informationen zu diesem und weiteren IIS-relevanten Registryschlüsseln gibt es auf einer [Support-Seite von Microsoft](http://support.microsoft.com/kb/954864).
