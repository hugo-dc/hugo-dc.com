---
title: wpm: A very simple "package manager" for Windows
---

About wpm
=========

I've been using GNU/Linux for a long time, I started with a Live CD of Knoppix,
it used the KDE Desktop environment, and had a lot of application ready to use,
one to explore the Mandelbrot Set, another one with cool effects using only
ASCII characters. I really liked the Linux environment, then I tried other
Linux Distributions such as Ubuntu, Debian, Fedora, ArchLinux, ElementaryOS,
OpenSuse. Tried different Desktop Environments like Gnome, KDE, dwm, XFCE, etc. 

Last year my desktop computer died, currently I only use the laptop that I use
to work, this laptop has Windows 7 installed, I still have a lot of tools that
I used to use when I had Linux, like Vim, Terminal, Custom Python scripts, etc. 

One of the things I missed the most of Linux is the ability to install programs
using the terminal, with help of a package manager. For example, If I want to
install Skype on Ubuntu, I don't have to open the Web Browser, find the Skype
web page, download the installer, and start installing the app (As you have to
do it in Windows). In Ubuntu you only type a single line in your terminal,
provide your admin password and is done.

```
$ sudo apt-get install skype
```

My daily work doesn't involve working with Git or Mercurial or any source code
version control system. But for side-projects I use Git to upload my code to
Github, to do this I use GitBash on Windows, GitBash is an amazing tool, it
provides a command line interface with a Bash interpreter and some GNU tools.
I haven't use bash for big projects, I used to write some bash lines in very
short scripts, I needed to explore more of bash, so I decided to create
a "package manager" for Windows, to install applications using the command
line. So I started a side project called `wpm` or Windows Package Manager, `wpm` 
is not really a package manager, it is like a installation helper, it automates
the process of searching the provider's web page, downloading the package and
opening the installer of the application. 

Installing and using wpm
========================

To install `wpm` you only have to type the following line in your Gitbash terminal:

```
curl -L https://raw.github.com/hugo-dc/wpm/master/install.sh | bash
```

The installer clones the `wpm` repository using Git (We are using Gitbash, so
Git is available!). The cloned repo is saved in `~\.bin\wpm`, in this path
exists a bash script called `wpm`, this path is added to your `PATH`
environment variable, then the `wpm` script is ready to use the next time you
start Gitbash, or if you reload your environment variables.

When you type the `wpm` command, it shows the following help:

```
$ wpm
__ __ __  _ __   _ __
\ V  V / | |_ \ | |  \
 \_/\_/  | .__/ |_|_|_|
         |_|
______________________________________________________________________________________
simple package manager for gitbash

Usage: wpm [option] [package]

Options
        search  [package]                - Search for packages | List packages
        install <package>                - Installs new package
        create  <package> [type]         - Types [default|github]
        update                           - Update wpm (git pull to main repository)

```

You can see a list of the available packages that you can install using `wpm`,
just type the command `wpm search`.

You can install a new package using `wpm install <package name>`.

Or you can create your own `wpm` package. There are two kind of packages,
Binary packages and the source code packages. 

Binary packages are the ones that when downloaded the system have to execute
the downloaded program and the installation starts.

Source code packages are packages that are downloaded from Github. Then after
downloading the package, some commands are needed to install it.

Here is an example of the package definition of the program `TeamViewer`:

```bash
#!/bin/bash

BINTYP="default"
BINURL="http://downloadus1.teamviewer.com/download/TeamViewer_Setup.exe" 
O_FILE="TeamViewer_Setup.exe" 
MD5="e819846551dcafac9c01d3171de05a89"
declare -a DEPEND=("")

function setup {
	echo "cmd //c $O_FILE ; " # put setup command here, end it with semicolon ;
}

function check_install {
    DEFINST="/C/Program Files (x86)/TeamViewer"   
    if [ -d "$DEFINST" ] ; then 
        echo "0"
    else 
        echo "1"
    fi
}

case "$1" in
	"bintype" )
		echo $BINTYP;;
	"binurl" )
		echo $BINURL;;
	"depend" )
		echo ${DEPEND[@]};;
	"o_file" )
		echo $O_FILE;;
	"md5" )
		echo $MD5;;
	"setup_command" )
		setup ;;
	"check_install" )
		check_install;;
esac

```

Here the setup function creates a `cmd` instance and execute the
`TeamViewer_Setup.exe` program.

Here is another example of a `Github` package:

```bash
#!/usr/bin/env bash

BINTYP="github"
BINURL="git://github.com/simplegeo/python-oauth2.git"
O_FILE="python-oauth2"
declare -a DEPEND=( "python27" "python-httplib2" )    # What happens if git is not in PATH?

function setup {
    echo "cd python-oauth2 ; "
    echo "python setup.py install ;"
    echo "cd - >> stderr ; "
}


function check_install {
    which python >> ~/.bin/wpm/log
    if [[ "$?" = "0" ]]; then 
        python -c 'exec "try: import oauth2\nexcept:exit(4)"'
    fi
    echo $?
}


case "$1" in 
    "bintype" )
        echo $BINTYP;;
    "binurl" )
        echo $BINURL;;
    "depend" )
        echo ${DEPEND[@]};;
    "o_file" ) 
        echo $O_FILE ;; 
    "setup_command" )
        setup ;; 
    "check_install" ) 
        check_install;;
    "author" ) 
        echo "@hugo_dc" ;; 
esac
```

In this case the github repository is cloned, and then the setup commands are
executed (see setup function).

You can create your own `wpm` package installer using the command `wpm create
[default|github]`.

[Here is a link the the Github repository of this project.](https://github.com/hugo-dc/wpm)



