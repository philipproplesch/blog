---
title: Almost cross-platform EncFS
date: 2015-11-08
tags:
  - security
  - encryption
---
As most people I have a whole bunch of data stored on *&lt;insert your preferred cloud storage provider here&gt;* and naturally some of these files contain sensitive data.

I am not paranoid and I am definitely not the kind of person who wears a tinfoil hat, but in times where pretty much every device is connected to the internet I sleep better knowing that my data belongs to me &mdash; **only me!**

## The easy way

The all-in-one solution would be [Boxcryptor](https://www.boxcryptor.com/en) which makes EncFS available to Windows and OSX users and even provides apps for OS X and Android.

## The hard way

Honestly I don't really need to access the files on the go, so smartphone or tablet access is of no priority for me right now.

### On OS X

In order to get EncFS up and running on OS X make sure that [Homebrew](http://brew.sh/) is installed on your machine.

Execute the following commands in a Terminal window and you are good to go:

    brew update
    brew install homebrew/fuse/encfs

After this you can start to use the `encfs` command:

    encfs ~/path/to/my/encrypted/data ~/decrypted

If you run this for the first time it will guide you through the initialization process. When you went through this before you just have to enter your password and can browse your files in `~/decrypted`.

When working with the decrpyted files in your folder you might run into this error when you try to open a file.

![File is damaged and can't be opened](/images/Screen-Shot-2015-11-08-at-7-13-49-PM.png)

Don't worry! Your file will be most likely OK, but OS X has added a flag which prevents it from getting opened. Running the following once in a while should resolve the issue:

    xattr -r -d com.apple.quarantine ~/decrypted

### On Windows

You will have support for EncFS on Windows, but it is in my opinion (significantly) poorer than on any *nix operation system.

In the past I went with [encfs4win](http://members.ferrara.linux.it/freddy77/encfs.html) (which requires [Dokan](http://dokan-dev.github.io/)), but my current favourite is [Safe](https://github.com/safeapp/safe).

It is an application with a simple GUI. Just point to the encrypted folder, enter the password and it will attach a WebDAV drive with the decrypted content.

Happy encrypting!
