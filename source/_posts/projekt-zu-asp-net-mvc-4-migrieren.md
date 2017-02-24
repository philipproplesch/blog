---
title: Projekt zu ASP.NET MVC 4 migrieren
date: 2012-02-12
tags:
  - asp.net mvc
---
Heute Vormittag hat Microsoft im Rahmen der [TechDays in Belgien](http://www.microsoft.com/belux/techdays/2012) die Beta-Version von [ASP.NET MVC 4](http://www.asp.net/mvc/mvc4) veröffentlicht. Allem Anschein nach beinhaltet diese Version eine [Go-Live-Lizenz](http://forums.asp.net/t/1770472.aspx/1?ASP+NET+MVC+4+Beta+Released), sodass sie auch problemlos in Produktivszenarien eingesetzt werden kann.

Um eine bestehende ASP.NET MVC-Applikation auf die neue Version zu aktualisieren sind folgende Schritte nötig:

Die web.config im Root-Verzeichnis der Applikation muss wie folgt erweitert bzw. modifiziert werden:

    <?xml version="1.0"?>
    <configuration>
      <appSettings>
        <add key="webpages:Version" value="2.0.0.0" ></add>
        <add key="webpages:Enabled" value="true" ></add>
        <add key="PreserveLoginUrl" value="true" ></add>
      </appSettings>
      <runtime>
        <assemblyBinding xmlns="urn:schemas-microsoft-com:asm.v1">
          <dependentAssembly>
            <assemblyIdentity name="System.Web.Helpers" publicKeyToken="31bf3856ad364e35" ></assemblyIdentity>
            <bindingRedirect oldVersion="1.0.0.0-2.0.0.0" newVersion="2.0.0.0" ></bindingRedirect>
          </dependentAssembly>
          <dependentAssembly>
            <assemblyIdentity name="System.Web.Mvc" publicKeyToken="31bf3856ad364e35" ></assemblyIdentity>
            <bindingRedirect oldVersion="1.0.0.0-4.0.0.0" newVersion="4.0.0.0" ></bindingRedirect>
          </dependentAssembly>
          <dependentAssembly>
            <assemblyIdentity name="System.Web.WebPages" publicKeyToken="31bf3856ad364e35" ></assemblyIdentity>
            <bindingRedirect oldVersion="1.0.0.0-2.0.0.0" newVersion="2.0.0.0" ></bindingRedirect>
          </dependentAssembly>
         </assemblyBinding>
      </runtime>
    </configuration>

In ALLEN web.config-Dateien, also auch jener im "Views"-Verzeichnis und ggfs. weiteren hinzugefügten, muss die Major-Version der folgenden Assemblies um eine Version inkrementiert werden:

    System.Web.Mvc, Version=3.0.0.0
    System.Web.WebPages, Version=1.0.0.0
    System.Web.Helpers, Version=1.0.0.0
    System.Web.WebPages.Razor, Version=1.0.0.0
    
    System.Web.Mvc, Version=4.0.0.0
    System.Web.WebPages, Version=2.0.0.0
    System.Web.Helpers, Version=2.0.0.0
    System.Web.WebPages.Razor, Version=2.0.0.0

Die MVC-relevanten Projektreferenzen müssen auf die neueste Version (in Klammern hinter dem Assemblynamen) aktualisiert werden:

    System.Web.Mvc (4.0.0.0)
    System.Web.WebPages (2.0.0.0)
    System.Web.Razor (2.0.0.0)
    System.Web.WebPages.Deployment (2.0.0.0)
    System.Web.WebPages.Razor (2.0.0.0)

Abschließend muss der Projekttyp der Applikation in der Projektdatei angepasst werden. Hierzu einfach innerhalb des Knotens "ProjectTypeGuids"die Guid `{E53F8FEA-EAE0-44A6-8774-FFD645390401}` durch `{E3E379DF-F4C6-4180-9B81-6769533ABE47}` ersetzen.
