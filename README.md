# QtStaticBuild

This repo stores instructions and script(s) for building static versions of Qt.

## Windows

The starting point is the article [Building a static Qt for Windows using MinGW](https://wiki.qt.io/Building_a_static_Qt_for_Windows_using_MinGW) which provides a link to the following PowerShell script: https://sourceforge.net/p/qtlmovie/code/ci/v1.2.16/tree/build/windows-build-qt-static.ps1?format=raw

This script was made for Qt 5.3 and so needs updating, but I have found it buggy. I have made some very small modifications to enable it to be run from the command line without any arguments. (Instead the script needs to be edited to set the default arguments to what you want.)

The key part of the script is the Qt configuration. Search for `cmd /c "configure.bat`. There were some problems with the whitespace in the original version that I have fixed. Note however that the original version uses the desktop version of OpenGL which I have found problematic on every version of Windows (option `-opengl desktop`). It also excludes SSL (option `-no-openssl`) which is fine as long as you're not accessing `https://` URLs.

### Notes on OpenSSL

If there's a need to communicate via https, we need to include OpenSSL support. The information on [Frederick Thomssen's blog](https://www.thomssen-it.de/blog/how-to-compile-qt-5-statically-with-mingw-including-openssl-on-windows) was valuable. Note that for Qt 5.7, although OpenSSL recommend installing both 1.1.0e and 1.0.2k, it would only work if 1.0.2k alone was installed. This is incorporated in commit 81b11d4.

The blog article can be summarised as follows:

1. Before you can compile Qt, you need to have OpenSSL installed. Either download & build [OpenSSL from their Github repo](https://github.com/openssl/openssl) or take a shortcut and use [Shining Light's prebuilt Windows binaries](https://slproweb.com/products/Win32OpenSSL.html). For Qt 5.7, install only version 1.0.2k (full version for Win32) and install it to the default location `C:\OpenSSL-Win32`.
1. Note edits made to the script in commit 81b11d4 which include replacing `-no-openssl` with `-openssl-linked` and including library paths. (That's why the default location was important in the previous step.)
1. As well as helping with OpenSSL support, there are tips about including `configure` directives to skip unneeded Qt modules. These didn't all work for me as at least one had been deprecated by Qt 5.7. I have included a reduced list that did work for me and which reduced build time by perhaps an hour.
1. Another tip was to temporarily disable antivirus while building as this can further reduce the build time.
1. Finally, the OpenSSL libraries are supported but not statically built into Qt by this method and so the following files should be copied from the `C:\OpenSSL-Win32` folder and included with any app compiled with this version of Qt:
    - libeay32.dll
    - libssl32.dll
    - ssleay32.dll

### Instructions for using the build script

1. Download a standard version of Qt using the [Qt Online Installer For Windows](http://download.qt.io/official_releases/online_installers/qt-unified-windows-x86-online.exe) (available from the [Download Qt Open Source](https://www.qt.io/download-open-source) page)
1. Install the version of Qt you want to use for standard use. This will be a dynamically linked version. I am assuming that you install this at `C:\Qt`. This will probably take a couple of hours to download and build.
1. Download the script `windows-build-qt-static.ps1` from this repo into the `C:\Qt` folder.
1. Assuming you need OpenSSL support, which commit 81b11d4 introduces, you'll need to install the OpenSSL library. See the notes on OpenSSL above.
1. Read the instructions in the opening comments. These relate to the version of Qt you wish to build and the location and version of the MinGW compiler, which you will probably need to edit. (I leave the `$QtVersion` parameter as empty string because the script works it out.)
1. Open Powershell
1. Navigate to the Qt folder: `cd C:\Qt`
1. Run the script: `./windows-build-qt-static.ps1`. This will take around 2-3 hours to build. It's worth checking occasionally in case it fails for some reason.
