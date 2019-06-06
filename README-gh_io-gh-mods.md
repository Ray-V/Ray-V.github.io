[<img src="https://ray-v.github.io/tde-slackbuilds/images/TDE-aarch64-gui.png" alt="" style="display:block;margin:auto;width:100%;max-width:600">](https://ray-v.github.io/tde-slackbuilds/images/TDE-aarch64-gui.png)
... a TDE desktop, cross compiled for aarch64, running on a RPi3.

---
***Build TDE [Trinity Desktop Environment]***  
for Slackware 14.2 or current on i586+, x86_64, or for Raspberry Pi3 [see [README-Raspberry-Pi3.md](./README-Raspberry-Pi3.md)], or cross compile for RPi3 armv7/aarch64 [see 'Cross compiling for RPi3'].

Build the release versions R14.0.4/5, or pre-release snapshot r14.0.6, from tar archives; or the development version from trinitydesktop cgit.

For a native build, run **./BUILD-TDE.sh** - a dialog based script with a series of screens for user input.  
The default is to install the packages as they are built, which is necessary initially for the required packages and for some interdependencies [for example, tdesdk requires tdepim].  
Run **INST=0 ./BUILD-TDE.sh** to build only.

Any package, or set of packages, can be selected in the 'TDE Packages Selection' screen.
The TDE mandatory packages can be pre-selected.
Notes at the bottom of the dialog screen have been added for information about dependencies for some packages.

The directory structure for the SlackBuild scripts is in line with the Trinity release source repositories:  
```
Deps [dependencies/]
Core []
Libs [libraries/]
Apps [applications/]
```
Other directories are:  
```
Misc - for non-Trinity package builds
src - to hold all the source tarballs, either pre-downloaded or downloaded during the build.
```
Other scripts:  
```
get-source.sh - a chunk of common code for the SlackBuilds - used for getting the source archive,  
                setting FLAGS, creating build directories.
```
There is an override in the Misc SlackBuilds for non-trinity source archive URLs. Non-trinity builds have been included where a TDE package requires a dependency that is not in Slackware, or where it's an alternative to a TDE package.

Required packages for a basic working TDE are:  
```
Deps/tqt3
Deps/tqtinterface
Deps/arts
Deps/dbus-tqt
Deps/dbus-1-tqt
Deps/tqca-tls
Deps/libart-lgpl
Core/tdelibs
Core/tdebase
```
i18n support [locale and html/help docs] in the packages is restricted to whatever is selected in the BUILD-TDE.sh 'Select Additional Languages' screen and, of that, to whatever is available in any individual package source.

See https://wiki.trinitydesktop.org/How_to_Build_TDE_Core_Modules for more information

---

***Building a pre-release from snapshot sources***

This is similar to the build for a release version, but the sources are taken from the TDE cgit repository @ https://git.trinitydesktop.org/cgit/{package}.

---

***Building the development version from git sources***

The individual TDE apps can be cloned from Trinity git, so the build is set up to do that - except for individual language packs of tde-i18n. The whole tde-i18n download is ~1x10^6 bytes, so to reduce that, wget is used to download individual tde-i18n-$lang packs as they are not git repositories.

Once any git repository has been cloned, further downloads are updates only[2], giving the best options - only fetching what is needed, and incremental updates.

The git repositories are cloned to 'src/cgit'

---

***Cross compiling for RPi3***

There is a work-in-progress for cross compiling TDE for the Raspberry Pi3 based on these scripts - see the html page in the gh-pages branch:
```
git clone https://github.com/Ray-V/tde-slackbuilds.git  
cd tde-slackbuilds  
git checkout gh-pages
```

or @ https://ray-v.github.io/tde-slackbuilds/cross-compiling-TDE-for-the-RPi3.html

Includes:
* Setting parameters for a 32-bit [armv7] hard float, or 64-bit [aarch64], build
* Building a cross compiler toolchain
* Building a 64-bit kernel which can be used for the 32-bit system
* Building qemu to run the TDE binaries built and used during compilation
* The required TDE apps
* ... and a few other TDE and non-TDE apps

---

***Known issues***

[1] TDM may need some manual setting up - see Core/tdebase/README, which can also be viewed while running BUILD-TDE.sh if tdebase is selected.

[2] The i18n downloads with wget can't be updated because cgit produces 'current time' timestamps. The consequence is that if tde-i18n-$lang is a part of the build after its initial download, it will be downloaded again. As updates are infrequent, once built, there will probably be no need to do so again and so tde-i18n for a particular language will probably only be run once. On that basis I don't see this being a significant issue.

[3] If Slackware's KDE is installed as well as TDE, there might be an issue with TDE launching the KDE4 Konsole and attempting to use it's ark. To fix, adjust the PATH so that the TDE directories come before /usr/share/bin.

[4] The Misc directory contains SlackBuilds for software that might already be installed from other sources. Please check because any misc builds selected here could overwrite them.
