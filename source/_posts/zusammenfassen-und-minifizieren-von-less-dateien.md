---
title: Zusammenfassen und Minifizieren von .less-Dateien
date: 2012-03-10
tags:
  - asp.net
  - asp.net mvc
---
Vor ein paar Tagen habe ich hier über die neuen Möglichkeiten zur Zusammenfassung und Minifizierung von JavaScript- und CSS-Dateien mittels der neuen [System.Web.Optimization-Bundles](http://msdn.microsoft.com/en-us/library/system.web.optimization.bundle(v=vs.110).aspx) geschrieben.

Grundsätzlich eine feine Sache, allerdings werden lediglich CSS und JavaScript unterstützt. [Sass](http://sass-lang.com/), [LESS](http://lesscss.org/), [CoffeeScript](http://coffeescript.org/), etc.? Fehlanzeige!

Glücklicherweise lassen sich weitere [BundleTransform](http://msdn.microsoft.com/en-us/library/system.web.optimization.ibundletransform.aspx)er recht simpel implementieren.

Hier ein Beispiel für LESS:

    public class LessTransform : IBundleTransform
    {
        public void Process(BundleContext context, BundleResponse response)
        {
            response.ContentType = "text/css";
            response.Content = Less.Parse(response.Content);
        }
    }
    
    public class LessMinify : CssMinify
    {
        public override void Process(BundleContext context, BundleResponse response)
        {
            new LessTransform().Process(context, response);
            base.Process(context, response);
        }
    }

Die Verwendung sieht wie folgt aus:

    protected void Application_Start()
    {
        AreaRegistration.RegisterAllAreas();
    
        RegisterGlobalFilters(GlobalFilters.Filters);
        RegisterRoutes(RouteTable.Routes);
    
    #if DEBUG
        var bundle   = new Bundle("~/Content/css", new LessTransform());
    #else
        var bundle = new Bundle("~/Content/css", new LessMinify());
    #endif
    
        bundle.AddFile("~/Content/Site.css", false);
        bundle.AddFile("~/Content/Forms.less", false);
        bundle.AddFile("~/Content/Another.less", false);
    
        BundleTable.Bundles.Add(bundle);
    }

Zum parsen der LESS-Dateien verwende ich [.less (dotless)](http://www.dotlesscss.org/).
