## THIS IS A FORK OF `diff-pdf` THAT USES NO GRAPHICAL DISPLAY
This makes it suitable for headless operations like running PDF comparison tests on a Docker container.

*Note: this repository is provided **as-is** and the code is not being actively
developed. If you wish to improve it, that's greatly appreciated: please make
the changes and submit a pull request, I'll gladly merge it or help you out
with finishing it. However, please do not expect any kind of support, including
implementation of feature requests or fixes. If you're not a developer and/or
willing to get your hands dirty, this tool is probably not for you.*

[![Build status](https://ci.appveyor.com/api/projects/status/m6d8n2kcyvk3cqi6?svg=true)](https://ci.appveyor.com/project/vslavik/diff-pdf)

## Usage

diff-pdf is a tool for visually comparing two PDFs.

It takes two PDF files as arguments. By default, its only output is its return
code, which is 0 if there are no differences and 1 if the two PDFs differ. If
given the `--output-diff` option, it produces a PDF file with visually
highlighted differences:

```
$ diff-pdf --output-diff=diff.pdf a.pdf b.pdf
```

See the output of `$ diff-pdf --help` for complete list of options.

## Obtaining the binaries

Precompiled version of the tool for Windows is available as part of
[the latest release](https://github.com/vslavik/diff-pdf/releases/tag/v0.4.1)
as a ZIP archive, which contains everything you need to run diff-pdf. It will
work from any place you unpack it to.

On Mac, if you use [Homebrew](https://brew.sh), you can use it to install diff-pdf with it:
```
$ brew install diff-pdf
```
On Mac, if you use [Macports](https://macports.org), you can install diff-pdf with:
```
$ port install diff-pdf
```
On  Fedora and CentOS 8:
```
$ sudo dnf install diff-pdf
```
Precompiled version for openSUSE can be downloaded from the
[openSUSE build service](http://software.opensuse.org).


## Compiling from sources

The build system uses Automake and so a Unix or Unix-like environment (Cygwin
or MSYS) is required. Compilation is done in the usual way:

```
$ cmake -S <source-dir> -B <build-dir> -DCMAKE_BUILD_TYPE=Release
$ cmake --build <build-dir>
$ cmake --install <build-dir>
```

As for dependencies, diff-pdf requires the following libraries:

- Cairo >= 1.4
- Poppler >= 0.10

#### Ubuntu (Dockerfile)
```
RUN apt-get install -y make automake cmake g++ libpoppler-glib-dev poppler-utils wxgtk3.0-dev git
RUN mkdir diff-pdf
WORKDIR /usr/src/app/diff-pdf
RUN git clone --depth=1 https://github.com/ggilles/diff-pdf.git .
RUN mkdir bin
RUN cmake -v -S . -B bin -DCMAKE_BUILD_TYPE=Release
RUN cmake --build bin
RUN cmake --install bin
RUN cp bin/diff-pdf /usr/local/bin
```

#### CentOS:

```
$ sudo yum groupinstall "Development Tools"
$ sudo yum install wxGTK wxGTK-devel poppler-glib poppler-glib-devel
```

#### Ubuntu:

```
$ sudo apt-get install make automake g++
$ sudo apt-get install libpoppler-glib-dev poppler-utils libwxgtk3.0-dev
```


#### macOS:
Install Command Line Tools for Xcode:

```
$ xcode-select --install
```

and install [Homebrew](https://brew.sh) or [MacPorts](https://www.macports.org) to manage dependencies, then:

```
$ brew install automake autoconf wxmac poppler cairo pkg-config
```

or

```
$ sudo port install automake autoconf wxWidgets-3.0 poppler cairo pkgconfig
```

Note that many more libraries are required on Windows, where none of the
libraries Cairo and Poppler use are normally available. At the time of writing,
transitive cover of the above dependencies included fontconfig, freetype, glib,
libpng, pixman, gettext, libiconv, libjpeg and zlib.


### Compiling on Windows using MSYS + MinGW

1. First of all, you will need working MinGW installation with MSYS2 environment
and C++ compiler. Install MSYS2 by following [their instructions](https://www.msys2.org).

1. Once installed, launch the MSYS2 MinGW shell. It will open a terminal window;
type `cd /c/directory/with/diff-pdf` to go to the directory with diff-pdf
sources.

1. You will need to install additional MSYS components that are not normally
included with MSYS, using these commands:

    ```
    $ pacman -Syu
    $ pacman -S automake autoconf pkg-config make zip
    $ pacman -S mingw-w64-i686-{gcc,poppler,wxWidgets}
    ```

1. Build diff-pdf in the same way as in the instructions for Unix above:

    ```
    $ ./bootstrap  # only if building from git repository
    $ ./configure
    $ make
    ```

1. To build a ZIP archive will all DLLs, run
    ```
    $ make windows-dist
    ```


## Installing

On Unix, the usual `make install` is sufficient.

On Windows, installation is not necessary, just copy the files somewhere. If
you built it following the instructions above, all the necessary files will be
in the created ZIP archive.
