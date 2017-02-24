---
title: Was tun, wenn NuGet down ist?
date: 2012-03-09
tags: nuget
---
![NuGet not available](/images/nuget_down.png)

Momentan sind weder die [offizielle NuGet-Seite](http://nuget.org/), noch der [Package-Feed](https://nuget.org/api/v2/) erreichbar. Wer auf keinen privaten oder lokalen Feed zurückgreifen kann, wird also warten müssen bis die Dienste wieder verfügbar sind. Oder etwa doch nicht?

NuGet speichert alle heruntergeladenen Pakete im Verzeichnis `%LocalAppData%/NuGet/Cache`. Wenn dieser Pfad als zusätzliche *Package Source* im Visual Studio eingetragen wird, besteht zumindest die Möglichkeit bereits heruntergeladene Packages zu installieren.

Alternativ kann auf den Dienst [MyGet](http://www.myget.org/) zurückgegriffen werden. Ich habe ihn bis dato nicht getestet, aber es handelt sich wohl um eine Art "NuGet as a Service".
