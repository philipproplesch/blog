---
title: Let Grunt install your dependencies
date: 2013-12-13
tags:
  - javascript
  - grunt
  - ci
---
I am currently working on a [Single Page Application](http://en.wikipedia.org/wiki/Single-page_application) and use [Grunt](http://gruntjs.com/) for build and task automation. Since new dependencies are added from time to time (either through `npm` or `bower`), we also have to make sure that these are present on the CI server.

I am not a huge fan of configuring the build in multiple places (build script, CI server, here and there), therefore I have created the following tasks which make use of the [`exec` method](http://nodejs.org/api/child_process.html).

    // http://gruntjs.com/creating-tasks#why-doesn-t-my-asynchronous-task-complete
    var executeCommand = function(command, callback) {      
      var exec = require('child_process').exec;

      exec(command, function(error, stdout, stderr) {
        console.log(stdout);
        callback();
      });
    };

    grunt.registerTask('install-npm-dependencies', function() {
      var done = this.async();
      executeCommand('npm install', done);
    });

    grunt.registerTask('install-bower-dependencies', function() {
      var done = this.async();
      executeCommand('bower install', done);
    });

Afterwards your can add them your actual build task.

    grunt.registerTask('continous-build', [
      'install-npm-dependencies',
      'install-bower-dependencies',
      'build'
    ]);
