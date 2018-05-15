---
layout: "post"
title: "Oracle Commerce Endeca 11.2 Silent Installation"
categories: oracle-commerce endeca
---

I had some issues getting the silent installation to work for the new `Endeca` installations for the newly released 11.2.

Oracle have been upgrading the installer processes with each release lately. As of 11.2 `MDEX`, `CAS`, and `PlatformServices` now all use the same `InstallAnywhere` process as does `Oracle Commerce (ATG)`. `ToolsAndFrameworks` uses a different format.

The `Endeca` documentation contains references to each installation guide for `MDEX`, `PlatformServices`, `ToolsAndFrameworks` and `CAS`.

`MDEX` silent install correctly refers to the `USER_INSTALL_DIR` configuration but it was the first one to change from a shell script to binary executable installation in a previous release.

`PlatformServices` and `CAS` have changed in 11.2 but incorrectly specify `INSTALLDIR` instead of `USER_INSTALL_DIR`.

To get the full set of InstallAnywhere installation options you can run with the `-r` option specifying the output response file whilst running in interactive mode. This will write out the options configuration after the interactive installation which you can then tailor for your silent installation.

In addition the `CAS` silent install examples show:

`INSTALLDIR=C:/Endeca/CAS`

whereas in fact you should just put the root path e.g. `/opt` as the installation will add the `endeca/CAS` directories and you would end up with duplicated directories, i.e.

`USER_INSTALL_DIR=/opt/endeca/CAS would result in /opt/endeca/CAS/endeca/CAS`

So as always its probably best to perform an interactive installation with the `-r` option, then iteratively try silent installation variations until you have the desired outcome.

I will hopefully be adding a `Chef` cookbook for `Endeca` installation to the Chef Marketplace shortly.