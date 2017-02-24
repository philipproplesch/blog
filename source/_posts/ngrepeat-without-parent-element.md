---
title: ngRepeat without parent element
date: 2014-03-30
tags: angularjs
---
I'm working on an [AngularJS](http://angularjs.org/)-driven Single Page Application for a couple of months now and have to admit that it's a framework I really enjoy working with. That does not happen very often ;)

---

Imagine you want to repeat a list of a certain type (posts, comments, whatever).

With AngularJS you usually make use of the built-in [ngRepeat directive](http://docs.angularjs.org/api/ng/directive/ngRepeat) as in the following example:


    <article ng-repeat="post in posts">
      <h1 ng-bind="post.title" />
      <!-- ... -->
    </article>
    
The ouput will look similar to this:

    <article>
      <h1>Hello world!</h1>
      <!-- ... -->
    </article>
    
    <article>
      <h1>AngularJS is awesome!</h1>
      <!-- ... -->
    </article>
    
    <article>
      <h1>I see dead people :/</h1>
      <!-- ... -->
    </article>
    
    <!-- and so on... -->
    
As you might noticed the ngRepeat directive sits on the `<article />` element which is great in most cases, but could be a problem if you don't have a parent element.

Let's take a [definition list](http://html5doctor.com/the-dl-element/) with the following markup:

    <dl>
      <dt>Foo</dt>
      <dd>Lorem ipsum dolor sit amet, consectetur adipisicing elit.</dd>
      <dt>Bar</dt>
      <dd>Iste aperiam nesciunt enim dolorum dolores labore necessitatibus dolor?</dd>
    </dl>

`<dt />` and `<dd />` are the elements that should be repeated now, but putting the ngRepeat on the `<dl />` wouldn't make much sense because it will generate *n* separate lists (n = length of array).

Fortunately there's ~~an app~~ a [solution for this in the framework](http://docs.angularjs.org/api/ng/directive/ngRepeat#special-repeat-start-and-end-points) :)

    <dl>
      <dt ng-repeat-start="definition in definitions" ng-bind="definition.title"></dt>
      <dd ng-repeat-end="" ng-bind="definition.text"></dd>
    </dl>

Et voilà! See it in action on [JSFiddle](http://jsfiddle.net/philipproplesch/SHjy9/).
