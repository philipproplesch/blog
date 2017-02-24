---
title: "System.Web.Optimization: Bundles in der web.config registrieren"
date: 2012-04-12
tags:
  - asp.net
  - asp.net mvc
---
Ich habe in den letzten Tagen erstmals das "[Bundling & Minification](http://weblogs.asp.net/scottgu/archive/2011/11/27/new-bundling-and-minification-support-asp-net-4-5-series.aspx)"-Feature in einem Projekt verwendet und bin bis dato eigentlich auch sehr zufrieden damit. Etwas unpraktisch finde ich allerdings die Tatsache, dass sämtliche Bundles hart im Code verdrahtet werden müssen.

Auf GitHub findet sich nun ein kleines [Repository](https://github.com/philipproplesch/Web.Optimization) von mir, welches die Registrierung der Bundles in der web.config ermöglicht:

    <configSections>
      <section name="web.optimization" type="Web.Optimization.Configuration.OptimizationSection" />
    </configSections>
    
    <web.optimization>
      <bundles>
        <bundle virtualPath="~/Content/css" transform="System.Web.Optimization.CssMinify, System.Web.Optimization">
          <content>
            <!-- Add some single files -->
            <add virtualPath="~/Content/Site.css" />
            <add virtualPath="~/Content/Forms.css" />
            <!-- Add a directory (and its subdirectories) -->
            <add virtualPath="~/Content/Styles" searchPattern="*.css" searchSubdirectories="true" />
          </content>
        </bundle>
      </bundles>
    </web.optimization>

Beim Start der Applikation müssen die Bundles lediglich via `RegisterConfigurationBundles()` registriert werden:

    protected void Application_Start()
    {
      // ...
    
      BundleTable.Bundles.RegisterConfigurationBundles();
    }

Vielleicht kann der ein oder andere ja etwas damit anfangen.
