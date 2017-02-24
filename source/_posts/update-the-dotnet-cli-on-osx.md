---
title: Update the dotnet CLI on OSX
date: 2016-05-22
tags:
  - .net core
  - cli
---
Microsoft has recently released new versions of .NET Core, ASP.NET Core and the development tools.

If you were playing around with earlier builds of the `dotnet` CLI and you want *(it is not really a question of whether you want or not ;))* to upgrade to the new version you have to remove all traces of any installed versions first.

Luckily Microsoft provides a simple script that we can execute as follows:

    # Get elevated privileges
    sudo su

    # Download and execute the uninstall script
    curl -s https://raw.githubusercontent.com/dotnet/cli/rel/1.0.0/scripts/obtain/uninstall/dotnet-uninstall-pkgs.sh | bash

Afterwards you can [download and install the latest package](https://dotnetcli.blob.core.windows.net/dotnet/preview/Installers/Latest/dotnet-dev-osx-x64.latest.pkg) which is linked on the [GitHub page](https://github.com/dotnet/cli/).

Executing `dotnet --info` or `dotnet --version` should return the correct version number now:

```
dotnet --info
.NET Command Line Tools (1.0.0-preview2-002820)

Product Information:
 Version:            1.0.0-preview2-002820
 Commit SHA-1 hash:  d307537eb8

Runtime Environment:
 OS Name:     Mac OS X
 OS Version:  10.11
 OS Platform: Darwin
 RID:         osx.10.11-x64
```
