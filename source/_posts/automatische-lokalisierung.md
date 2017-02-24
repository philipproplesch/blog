---
title: Automatische Lokalisierung
date: 2012-02-23
tags:
  - asp.net
  - asp.net mvc
---
Durch die verschiedenen [DataAnnotation-Attribute](http://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.aspx) ist die Lokalisierung bzw. Anzeige einer benutzerfreundlichen Beschriftung von Properties in ASP.NET-Anwendungen so einfach wie nie zuvor.

Model:
    
    public class FooViewModel
    {
        public string Bar { get; set; }
    }

View:

    <div class="control-group">        
        @Html.LabelFor(model => model.Bar)
        <div class="controls">            
            @Html.EditorFor(model => model.Bar)
            @Html.ValidationMessageFor(model => model.Bar)
        </div>
    </div>

Die Zeile `@Html.LabelFor(model => model.Bar)` sorgt dafür, das der Name der Property an diese Stelle gerendert wird:

    <div class="control-group">        
        <label for="Bar">Bar</label>
        <div class="controls">            
            ...
        </div>
    </div>

Falls für eine Property ein vom Namen abweichender Text angezeigt werden soll, kann dieser mittels über das [DisplayName-Attributs](http://msdn.microsoft.com/en-us/library/system.componentmodel.displaynameattribute.aspx) eingegeben werden:

    public class FooViewModel
    {
        [DisplayName("Foo")]
        public string Bar { get; set; }
    }

Soll der angezeigte Text abhängig von der aktuellen Sprache variieren, ist es möglich mit dem [Display-Attribut](http://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.displayattribute.aspx) auf Ressourcen zuzugreifen:

    public class FooViewModel
    {
        [Display(ResourceType = typeof(Labels), Name = "Foo")]
        public string Bar { get; set; }
    }

Möchte man auf sämtliche Attribute verzichten, aber dennoch schöne und vielleicht sogar lokalisierte Beschriftungen haben, besteht die Möglichkeit einen eigenen [ModelMetadataProvider](http://msdn.microsoft.com/en-us/library/system.web.mvc.modelmetadataprovider.aspx) zu implementieren bzw. vom bestehenden abzuleiten.

    public class LocalizedModelMetadataProvider
      : DataAnnotationsModelMetadataProvider
    {
        protected override ModelMetadata CreateMetadata(
            IEnumerable<Attribute> attributes,
            Type containerType,
            Func<object> modelAccessor,
            Type modelType,
            string propertyName)
        {
            if (string.IsNullOrEmpty(propertyName))
            {
                return base.CreateMetadata(
                    attributes,
                    containerType,
                    modelAccessor,
                    modelType,
                    propertyName);
            }
    
            var newAttributes = new List<Attribute>(attributes);
    
            if (newAttributes.All(
                x => x.GetType() != typeof(DisplayNameAttribute)))
            {
                var value = Labels.ResourceManager.GetString(propertyName);
    
                if (!string.IsNullOrEmpty(value))
                {
                    var attribute = new DisplayNameAttribute(value);
                    newAttributes.Add(attribute);
                }    
            }

            return base.CreateMetadata(
                newAttributes,
                containerType,
                modelAccessor,
                modelType,
                propertyName);
        }
    }

Hierbei wird für alle Properties, die bislang kein DisplayName-Attribut haben, ein korrespondierender String (Ressourcenname gleich Propertyname) aus der Ressourcendatei Labels geladen.

Die Registrierung eines eigenen ModelMetadataProvider erfolgt vorzugsweise im `Application_Start`-Event in der Global.asax.

    protected void Application_Start()
    {
        ModelMetadataProviders.Current =
            new LocalizedModelMetadataProvider();
    }
