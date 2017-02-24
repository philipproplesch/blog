---
title: Scripts und Stylesheets mit Boardmitteln minifizieren
date: 2012-03-06
tags:
  - asp.net
  - asp.net mvc
---
Ich möchte im folgenden NICHT über die Gründe schreiben, warum es sinnvoll ist Scripts oder Stylesheets zusammenzufassen. Macht es einfach! ;)

Mit der [Beta von ASP.NET MVC 4](http://www.asp.net/mvc/mvc4) (funktioniert bestimmt auch mit WebForms, aber wer will das schon?!) wurde der Namespace `System.Web.Optimization` ([Microsoft.Web.Optimization via NuGet](https://nuget.org/packages/Microsoft.Web.Optimization)) eingeführt und somit die Möglichkeit geschaffen JavaScript- und CSS-Dateien in ASP.NET-Anwendungen zu minifizieren und zu kombinieren (ähnlich wie [SquishIt](https://github.com/jetheredge/SquishIt), [Combres](https://github.com/buunguyen/combres) oder [Casette](http://getcassette.net/)).

Das `Application_Start`-Event wurde in den neuen Templates um die folgende Zeile erweitert:

    protected void Application_Start()
    {
        BundleTable.Bundles.RegisterTemplateBundles();
    }

Die Methode `RegisterTemplateBundles()` sorgt dafür, dass alle mitgelieferten Scripts (jquery, jquery-ui, jquery-validate, modernizr, …) und Stylesheets (Site.css) in den "Dateien" `~/Content/css` und `~/Scripts/js` zusammengefasst werden. Im Layout bzw. in der View erfolgt die Enbindung wie folgt:

    
    

Um ein Bundle im Nachhinein zu erweitern/modifizieren besteht die Möglichkeit über den virtuellen Pfad auf selbiges zuzugreifen:

    protected void Application_Start()
    {
        BundleTable.Bundles.RegisterTemplateBundles();
    
        var scripts = BundleTable.Bundles.GetBundleFor("~/Scripts/js");
    
        // Einzelne Datei hinzufügen
        scripts.AddFile("~/Scripts/knockout-2.0.0.js");
    
        // Alle *.js-Dateien im Ordner /Scripts/Subfolder und dessen Unterverzeichnissen hinzufügen
        scripts.AddDirectory("~/Scripts/Subfolder", "*.js", true);
    }

Als Alternative zu `RegisterTemplateBundles()` kann auch `EnableDefaultBundles()` verwendet werden. Hierbei werden die Dateien nicht explizit über den Namen zusammengefasst, sondern über die Dateierweiterung (*.js und *.css).

Bei der Verwendung von `RegisterTemplateBundles()` oder `EnableDefaultBundles()` werden die Dateien IMMER minifiziert und kombiniert. Falls die Minifizierung während der Entwicklungszeit verhindert werden soll (was zu Debuggingzwecken unter Umständen ganz hilfreich sein könnte ), sollte man mal schauen was [John Petersen heute zu diesem Thema geschrieben hat](http://codebetter.com/johnpetersen/2012/03/06/asp-net-mvc-4-beta-bundling-and-minification-dymystified/). Außerdem ist in dem Codebeispiel zu sehen wie Bundles selbst definiert werden können.

Standardmäßig werden die folgenden Dateien von der Kombinierung ausgeschlossen:

    IgnoreList.Ignore("*.intellisense.js");
    IgnoreList.Ignore("*-vsdoc.js");
    IgnoreList.Ignore("*.debug.js");

Weitere Informationen zu diesem Thema gibt es z.B. [bei Scott Guthrie](http://weblogs.asp.net/scottgu/archive/2011/11/27/new-bundling-and-minification-support-asp-net-4-5-series.aspx) oder auf den [offiziellen ASP.NET-Seiten](http://www.asp.net/web-forms/videos/aspnet-vnext/aspnet-vnext-videos-bundling-and-minification).
