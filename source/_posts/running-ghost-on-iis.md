---
title: Running Ghost on IIS
date: 2013-09-25
tags:
  - node.js
  - ghost
  - iis
---
Running [Ghost](http://ghost.org/) on IIS is fairly simple. First of all you have to make sure that [Node.js](http://nodejs.org/) and [iisnode](https://github.com/tjanczuk/iisnode) are installed on your machine.

Follow the installation instructions in Ghost's `README.md` and verify that the application pool has read and write permissions to the target directory.

Add a web.config file to your application root and insert the following lines, which tells iisnode to use `index.js` as the startup file. The rewrite rule ([URL Rewrite](http://www.iis.net/downloads/microsoft/url-rewrite) should be installed as well!) is required to pipe all incoming requests through the `index.js` file and let Ghost process them.

#### web.config

    <configuration>
      <system.webServer>
        <handlers>
          <add name="iisnode" path="index.js" verb="*" modules="iisnode" />
        </handlers>
        <defaultDocument enabled="true">
          <files>
            <add value="index.js" />
          </files>
        </defaultDocument>
        <rewrite>
          <rules>
            <rule name="Ghost">
    		  <match url="/*" />
              <conditions>
                <add input="{PATH_INFO}" pattern=".+\.js\/debug\/?" negate="true" />
              </conditions>          
              <action type="Rewrite" url="index.js" />
            </rule>
          </rules>
        </rewrite>
        <!--
          See https://github.com/tjanczuk/iisnode/blob/master/src/samples/configuration/web.config for information regarding iisnode specific configuration options.
        -->
        <iisnode node_env="%node_env%" loggingEnabled="false" debuggingEnabled="false" devErrorsEnabled="false" />
      </system.webServer>
    </configuration>
    

Last but not least you have to modify `./core/server.js` **OR** your `./config.js`, because we don't want to hook up another server on a different port beside IIS. Gladly iisnode provides the current port as an environment variable:

![iisnode port](/images/env_port_PNG.png)

#### ./config.js
    $environment: {        
    	server: {
    		host: '127.0.0.1',
    		port: process.env.PORT
    	}
    },

#### Original ./core/server.js


    // ## Start Ghost App
    server.listen(        
        ghost.config().server.port,
        ghost.config().server.host,
        function () {

#### Modified ./core/server.js

    // ## Start Ghost App
    server.listen(        
        typeof process.env['PORT'] !== 'undefined' 
            ? process.env.PORT 
            : ghost.config().server.port,
        ghost.config().server.host,
        function () {
        
        
---

#### DISCLAIMER:
Since I am not that familiar with Node.js, I am not sure if this is the way you should do it. Nevertheless it [works on my machine(s)](http://www.codinghorror.com/blog/2007/03/the-works-on-my-machine-certification-program.html) pretty good.
