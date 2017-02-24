---
title: Atlassian Stash - Add Pull Request reviewers automatically
date: 2014-04-26
tags:
  - bitbucket server
  - git
---
Though there are solutions to configure predefined Pull Request reviewers for a project in [Stash](https://www.atlassian.com/software/stash), this might become handy for people without the permissions to configure such a thing.

    // ==UserScript==
    // @name       Set reviewers for a pull request in Stash
    // @namespace  http://foo.bar/
    // @version    0.1
    // @description  
    // @match      */projects/*/repos/*/pull-requests?create
    // @copyright
    // ==/UserScript==

    document.getElementById('reviewers').value = 'johndoe|!|maxmustermann';    
    alert('Reviewers added...');
    
You just have to add the user names of the reviewers separated by `|!|`.

This will also work as a [bookmarklet](http://mrcoles.com/bookmarklet/).
