**Building TDE R14.0.4 on the Raspberry Pi3**

BUILD-TDE.sh has been set up with an option to build TDE on the Raspberry Pi3, with [Sarpi](http://sarpi.fatdog.eu/index.php?p=home) supplied kernel/modules/firmware.

All packages build on Slackware current and 14.2. There have been a number of patches to the source code for gcc 7.x.x, which is used on current.

Build times are as shown, with the number of parallel jobs set at 8 [1], and with one internationalization locale being included. The build was run with the top off the Pi casing, and the cpu temperature generally remained below 80 °C, occasionally peaking at about 82.5 °C. All four cpus ran @ 1200MHz [2].

<hr>

[1] This may not be the optimum. Based on a sample, builds may be quicker or slower at -j6, some with little difference at -j4, so YMMV. I assume this is because [performance is degraded](https://www.raspberrypi.org/documentation/configuration/config-txt/overclocking.md) at temperatures in excess of 80 °C.

[2] `echo ondemand > /sys/devices/system/cpu/cpu*/cpufreq/scaling_governor` if needed.

[3] Avahi-tqt needs libdaemon and avahi packages installed, and these can be built on the Pi from the SlackBuilds.org scripts.

[4] The inkscape build is memory intensive, and using too much swap slows the build. The best compromise is to build it separately and set NUMJOBS to -j3. ( [a] - inkscape-0.91, built with '-j1'/gcc-7.1.0/poppler-0.57 // [b] inkscape 0.92.2 cmake build with '-j3'/gcc-7.2.0/poppler-0.59.).

[5] Both inkscape 0.91 and tdegraphics builds on current with poppler 0.59 failed because of the introduction of a new Object API in poppler 0.58. The tdegraphics build was done with poppler 0.57.

**Build times**

<pre>
              <b>Current - hard float</b>     <b>Current - hard float</b>    <b>14.2 - soft float</b>
                without heatsinks         with heatsinks         with heatsinks

<b>Required</b>:
tqt3                   50:39                  45:52                  39:48
tqtinterface            4:26                   3:43                   2:11
arts                    6:47                   6:07                   5:37
dbus-tqt                  24                     26                     23
dbus-1-tqt                51                     45                     40
libart-lgpl               35                     35                     30
tqca-tls                  22                     21                     18
tdelibs                52:12                  49:32                  42:59
tdebase                54:47                  49:01                  44:02
                     -------                -------                -------
                     2:51:03                2:36:22                2:16:28

<b>Core</b>:
tdeutils               12:29                  10:57                  10:24
tdemultimedia          43:46                  42:13                  39:02
tdeartwork              4:53                   4:20                   4:01
tdegraphics            21:21                  21:14 [5]              17:59
tdegames               14:21                  12:43                  12:20
libcaldav               1:51                   1:20                   1:23
libcarddav              1:18                   1:13                   1:17
tdepim               1:00:09                  53:01                  47:20
tdeaddons               8:46                   8:19                   7:19
tdesdk                 24:38                  21:20                  18:37
tdevelop               41:40                  36:17                  31:32
tdetoys                 2:13                   2:09                   1:58
tdeedu                 36:07                  35:04                  34:31
tdewebdev              23:48                  23:26                  21:06
tidy-html5              1:19                   1:11                     59
speex                     47                     44                     42
tdenetwork             38:49                  33:12                  28:58
tdeadmin                6:31                   6:27                   5:50
tdeaccessibility       10:27                  10:41                   9:48
tde-i18n               13:42                  13:13                  12:41
                     -------                -------                -------
                     6:08:55                5:39:04                5:07:47

<b>Apps/Libs/Misc</b>:
GraphicsMagick          7:08                                          5:36
abakus                  1:05                                            53
avahi-tqt [3]           2:16                   1:35                   1:31
digikam                32:17                                         25:28
dolphin                 1:18                                          1:05
graphviz               21:36                                         17:34
gtk-qt-engine             46                                            39
gtk3-tqt-engine         2:35                                          2:31
inkscape [4]         3:11:11 [a]            1:33:28 [b]            1:29:09
k9copy                  5:27                                          5:22
kaffeine                6:28                                          6:19
kbfx                    2:03                                          1:47
kbookreader             1:45                                          1:40
kdbg                    2:26                                          2:11
kdbusnotification       1:36                                          1:36
kile                    5:40                                          5:22
kipi-plugins           13:02                                         13:10
knemo                   2:46                                          2:33
knights                 2:45                                          2:33
knmap                   2:09                                          2:07
koffice              3:40:32                                       3:24:30
koffice-i18n            1:08                                          1:10
krusader                8:23                                          8:19
kscope                  3:16                                          3:00
ksensors                2:24                                          2:17
kshutdown               2:24                                          2:19
ksquirrel               5:37                                          5:26
kvkbd                   1:51                                          1:41
kvpnc                   8:14                                          7:48
libksquirrel           11:54                                         11:53
libmp4v2                3:21                                          2:12
libpng                    39                                            31
lxml                    9:00                                          7:19
moodbar                   37                                            34
piklab                 19:18                                         16:24
potrace                 1:09                                            36
potracegui              1:53                                          1:46
rosegarden             28:04                                         19:35
soundkonverter          4:42                                          4:18
tde-style-lipstik       1:50                                          1:52
tde-style-qtcurve       1:25                                          1:19
tdeamarok               9:30                                          9:12
tdefilelight              49                                            43
tdegwenview             5:31                                          4:46
tdegwenview-i18n        2:20                                          2:27
tdeio-locate              37                                            35
tdek3b                  9:25                                          8:08
tdek3b-i18n               30                                            20
tdektorrent            13:04                                         12:17
tdelibkdcraw            2:42                                          2:34
tdelibkexiv2            1:51                                          1:52
tdelibkipi              2:25                                          2:19
tdesudo                 1:31                                          1:35
tdmtheme                1:35                                          1:36
twin-style-crystal      1:55                                          1:55
xmedcon                 1:58                                          1:46
yakuake                 2:09                                          2:08
yauap                      5                                             5
                    --------                                       -------
                    11:41:57                                       9:08:13

Overall total       20:41:55                                      16:32:28
</pre>
