---
title: Use IIS pickup directory with Nodemailer
date: 2013-12-28
tags:
  - node.js
---
In the ASP.NET ecosystem I often make use of the [IIS pickup directory](http://msdn.microsoft.com/en-us/library/ms164241(v=vs.110).aspx). It's easy, it's free. Period.

When installing [Ghost](https://ghost.org/) you will always get bothered by a message on the login screen saying something like `mimimi... no smtp server configured`.

Unfortunately [Nodemailer](http://www.nodemailer.com/), the mailing library used by Ghost, does not support my beloved pickup directory and a 3rd party service was not very satisfying. It's just a simple blog!

Well... I [forked](https://github.com/philipproplesch/Nodemailer) the repository and introduced a new [transport object](https://github.com/philipproplesch/Nodemailer/blob/master/lib/engines/pickup-directory.js) which stores the generated mail in a directory of your choice :)

### Update

As of version 0.6.0 it is part of Nodemailer. Have a look at the [README](https://github.com/andris9/Nodemailer/blob/master/README.md#setting-up-pickup) for details.

    var transport = nodemailer.createTransport('PICKUP', {
      directory: 'path/to/mails'
    });
    
    // or
    
    var transport = nodemailer.createTransport('PICKUP', 'path/to/mails');

## Configure Ghost to use the pickup directory

After creating a backup and applying the necassary changes, this is how Ghosts's `config.js` looks on my machine:

    production: {
      url: 'http://philipproplesch.de',
      mail: {
        transport: 'PICKUP',
        options: {				
          directory: 'path/to/mails'
        }
      },
      ...

Here is a mail that has been sent via the pickup directory after requesting a new password:

![Password reset mail](/images/ghost_pw_reset_PNG.png)
