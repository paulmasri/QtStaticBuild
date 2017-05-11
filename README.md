# QtStaticBuild

This repo stores instructions and script(s) for building static versions of Qt.

## Windows

The starting point is the article [Building a static Qt for Windows using MinGW](https://wiki.qt.io/Building_a_static_Qt_for_Windows_using_MinGW) which provides a link to the following PowerShell script: https://sourceforge.net/p/qtlmovie/code/ci/v1.2.16/tree/build/windows-build-qt-static.ps1?format=raw

This script was made for Qt 5.3 and so needs updating, but I have found it buggy. I have made some very small modifications to enable it to be run from the command line without any arguments. (Instead the script needs to be edited to set the default arguments to what you want.)

The key part of the script is the Qt configuration. Search for `cmd /c "configure.bat`. There were some problems with the whitespace in the original version that I have fixed. Note however that the original version uses the desktop version of OpenGL which I have found problematic on every version of Windows (option `-opengl desktop`). It also excludes SSL (option `-no-openssl`) which is fine as long as you're not accessing `https://` URLs.

### Instructions

1. Download a standard version of Qt using the [Qt Online Installer For Windows](http://download.qt.io/official_releases/online_installers/qt-unified-windows-x86-online.exe) (available from the [Download Qt Open Source](https://www.qt.io/download-open-source) page)
1. Install the version of Qt you want to use for standard use. This will be a dynamically linked version. I am assuming that you install this at `C:\Qt`. This will probably take a couple of hours to download and build.
1. Download the script `windows-build-qt-static.ps1` from this repo into the `C:\Qt` folder.
1. Read the instructions in the opening comments. These relate to the version of Qt you wish to build and the location and version of the MinGW compiler, which you will probably need to edit. (I leave the `$QtVersion` parameter as empty string because the script works it out.)
1. Open Powershell
1. Navigate to the Qt folder: `cd C:\Qt`
1. Run the script: `./windows-build-qt-static.ps1`. This will take around 5 hours to build. It's worth checking occasionally in case it fails for some reason.
