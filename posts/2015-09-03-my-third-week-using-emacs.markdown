---
title: My third week using Emacs
---

This is my third week using Emacs, I was using Vim since 2013. I knew
Emacs and Vim where very popular code editors, so I started using Vim
I really liked it, I was very comfortable editing code,
writting my blog posts (markdown files), writting my notes, even I
created a little Vim Plugin (written in Python) for developing ABAP
programs directly in Vim and submit code to SAP.

But I needed to know the other editor,is it
better? or just the same?.

So this is my experience using Emacs in laptop running Windows 7
during this three weeks:

--------------------------------------------------------------------------


Installing Emacs
====================

**The Unnoficial Installation**

You can install a modified version of GNU Emacs from
[Vincent Goulet](http://vgoulet.act.ulaval.ca/en/emacs/windows/)
website.

**I choosed this version** because it contains a lot of useful things,
it allows you to open PDF's files in Emacs without install any other
program (like MikTex). It's amazing that Emacs can show you a PDF
file embedded, I can read a PDF while taking notes!. Unfortunately it
doesn't work for large PDF files, It freezes Emacs, I will try to do
it in a linux box later.

To view PDF docs in Emacs you need GhostScript, you can get
GhostScript with Cygwin, run the Cygwin installer and select the
Ghostscript package.

Then you have to configure Emacs to be able to find the Ghostscript
program:

```emacs
M-x
customize-group
doc-view
```

**The Official Emacs**

You can download the more recent version of Emacs for windows [HERE](http://ftp.gnu.org/pub/gnu/emacs/windows/). It is not an installer, it's just a `zip` file. You can extract it wherever you want.

After extracting all files run `runemacs.exe`, this program will create
shortcuts and registry entries.

--------------------------------------------------------------------------

Configuring Emacs
=====================

You can customize Emacs using your `~/.emacs` file, in Windows the default home
for emacs is `%HOMEPATH%\AppData\Roaming\`. You can change it creating a new
environment variable called `%HOME%` (note that it's different from Windows'
home environment variable which is `%HOMEPATH%`). Then Emacs will detect your
`~/.emacs` file. 

In this `.emacs` file you can configure your emacs, to confiugre Emacs
you have to use Emacs Lisp. You can create your own functions and call
them using `M-x` or setting a keyboard shortcut.

Using Bash as shell
-------------------------

If you have Cygwin or Gitbash installed in your computer, you can use bash
instead of cmd.exe as shell in Emacs. To do this add the following code to your
`~/.emacs` file:

```clojure
(if (file-directory-p "C:/Path/To/Your/Cygwin/bin")
    (add-to-list 'exec-path "C:/Path/To/Your/Cygwin/bin"))

(setq shell-file-name "bash")
(setq explicit-shell-file-name shell-file-name)
(setq explicit-bash-args '("--login" "-i"))
```

Sometimes it may cause problems using Bash instead of `cmd.exe`. For
example, some Emacs packages detects that you are running Windows and
try to execute a specific Windows command which bash can't process
(I got a problem with [Cider](https://github.com/clojure-emacs/cider).

I really like the idea of being able to have a terminal embedded in my
editor, it makes me feels like in [dwm](http://dwm.suckless.org/) or
[xmonad](http://xmonad.org/). The only problem I had using Bash
embedded in Emacs, is that when I run certain commands (specially
`Git` commands) the terminal stopped working, I still could write there
or move to another window, but the terminal just stop working.
I hope this doesn't happen on Linux.

--------------------------------------------------------------------------

Using Emacs
==============

When you first open you first file in Emacs, you can start editing
right away. And you can use it as another "normal" editor, use the
menu to Save, Open Files, etc.

But, it is more useful when you learn a few keystrokes. Here are the
basics you need to know (Remember M (Meta) = Alt | C = Ctrl ):


**Moving** 

Keys     | Result
---------|---------------------------------------
C-a      | Begin of line
C-e      | End of line
C-f      | Move forward (next character)
C-b      | Move backwards (previous character)
C-n      | Next line
C-p      | Previous line
M-<      | Go to the beginning of current buffer
M->      | End of buffer
M-g g    | Go to specific line number
C-v      | Go to next "page"/screen
M-v      | Go to previous screen

**Edit**

Keys     | Result
---------|---------------------------------------
C-<SPC>  | Start selecting
M-w      | Copy selected region to "Kill ring"
C-y      | Yank (paste)
M-y      | Yank (paste), takes the prev element in kill ring
C-k      | Kill (cut) the text after the point (cursor)
C-x C-f  | Visit (open) file
C-x C-s  | Save
C-x C-c  | Quit

**Buffers**

Keys     | Result
---------|---------------------------------------------
C-x 3    | Split current buffer in two vertical buffers
C-x 2    | Split buffer horizontally
C-x 0    | Close buffer (it remains active)
C-x 1    | Close all other buffers
C-x b    | Open buffer
C-x C-b  | Show buffer list
C-x o    | Move to the next buffer
C-x r m  | Bookmark current buffer
C-x r b  | Open bookmarked buffer



**Others**

Keys   | Result
-------|---------------------------------------
C-h    | Help (press ? to see your options)
M-x    | Execute command
C-g    | Cancel (for example when you start M-x)

`C-h` is very useful, for example, if you press `C-h` and then:

Keys   | Result
-------|---------------------------------------
t      | Built in tutorial
b      | Keybindings available in current buffer
k      | Help on keystrokes
f      | Help on function
l      | Show the **l**ast 300 keystrokes

Last option is useful when you did something unnintentionally by
pressing some keys (and you want to know which keys).

--------------------------------------------------------------------------

.emacs
-------------

I really liked Emacs Lisp as the language to extend Emacs, create your
own custom functions is very easy. Here is what I've learned about
Lisp and some useful functions that may help you to configure your
`.emacs` file.

* Add a directory to the Emacs execution path:

```clojure
(add-to-list 'exec-path "C:/My/Path")
```

* Use a different Browser (to open Links in Emacs buffers) than the
system's default:

```clojure
(setq browse-url-browser-function 'browse-url-generic
      browse-url-generic-program "chrome.exe")
```

To do this your, Emacs has to know the location of your `chrome.exe`
program, so you have to add that path to the Emacs execution path (see
previous example).

* Set Tab width:

```clojure
(setq tab-width 4)
```

* Show line numbers in all buffers:

```clojure
(global-linum-mode 1)
```

* Maximize Emacs (when Emacs is starting):

```clojure
(add-to-list 'default-frame-alist '(fullscreen . maximized ))
```

* Avoid startup message:

```clojure
(setq inhibit-startup-message t
   inhibit-startup-echo-area-message t)
```
   
* Hide Toolbar:

```clojure
(tool-bar-mode 0)
```

* Hide scrollbars:

```clojure
(scroll-bar-mode 0)
```


By default, Emacs generates backup files, these files are named as the
original file but appending a `~` symbol, these files are generated in
the same directory where the original exists, you can change the path
where the backyp file is generated using:

```clojure
(setq backup-directory-alist `(("." . "~/.saves")))
```
You have to create the `.saves` directory in your %HOME% directory.

* Change default font:

```clojure
(set-default-font "Liberation Mono-11")
```

* Add package repositories:

```clojure
(require 'package)
(add-to-list 'package-archives
	     '("melpa" . "http://melpa.org/packages/"))
(add-to-list 'package-archives
	     '("marmalade" . "http://marmalade-repo.org/packages/"))
(add-to-list 'package-archives
	     '("tromey" . "http://tromey.com/elpa/"))

(package-initialize) ;;
```

You can find packages using: `M-x` and the function:
`package-list-packages`.

* Loading themes

You can customize the Emacs theme running command:

````
M-x
customize-themes
```

You can install new themes using package install, currently I'm using
`monokai` theme. This theme have to be loaded in your `.emacs` file
too:

```clojure
(load-theme 'theme-name)
```

Sometimes Emacs automatically adds a function `(custom-set-faces)`, if
you set your `load-theme` function **before**, Emacs will ask you if
you really want to use your theme, to avoid this, you have to place
your `load-theme` function after `custom-set-faces:

```clojure
(custom-set-faces
 ;; custom-set-faces was added by Custom.
 ;; If you edit it by hand, you could mess it up, so be careful.
 ;; Your init file should contain only one such instance.
 ;; If there is more than one, they won't work right.
 )

;; Load default theme
;; Dark themes:
(load-theme 'monokai)
```


* You can define your own Emacs functions following the next structure:

```clojure
(defun my-function-name ()
  "My Function description"
  (interactive)
  ;; logic
  )
```

`(interactive)` allows you to call the function using the `M-x`
command.

Here is an example of a function I made:

```clojure
(defun screencap ()
  "Captures an image and generates the required Markdown"
  (interactive)
  (shell-command "c:/users/hugo/workspace/notes/bnsc.bat")
  (sleep-for 3)
  (insert "![](/images/")
  (yank)
  (insert ".png)\n\n")
  )
```

I defined a function called screncap, it is **`interactive`** so, I can
call it using **`M-x`**. It executes a **`shell-command`**, in this case is a
BAT file, this BAT files calls a program, using this program I can
capture a region in the screen, when I'm done the program stores the
image in **"`/notes/images/`"** the name of the new generated image is
stored in the windows clipboard, when the program finished, the Lisp
function continues, it waits 3 seconds (**`sleep-for 3`**) and then
**`insert`** the string **"`![](/images/`"** then `yank` the content of the
clipboard and then adds the image extension and two more lines. This
way, when I'm editing a markdown note I can capture a screenshot and
the markdown code is generated with the correct image name.

I binded this function to the `C-s s` keystrokes using:

```clojure
(global-set-key (kbd "M-s s") 'screencap)
```

Here is a list of functions that you can use when constructing your
own functions:

Function                  |  Description
--------------------------|--------------------------------------------------
(interactive)             | Can be called using `M-x` or keystroke
(forward-char <n>)        | Move forward `n` characters
(delete-char <n>)         | Delete `n` characters to the right
(setq <name> <val>)       | "Assign" the value <val> to <name>
(search-forward <st>)     | Search forward string <st>
(buffer-substring <i> <e>)| Get substring starting from position <i> to <e>
(concat <st1> <st2>)      | Concat string <st1> with <st2>
(message <st>)            | Show message <st> in minibuffer
(insert <st>)             | Insert <st> in current buffer
(beginning-of-line)       | Move to the beginning of the current line
(end-of-line)             | Move to end of current line
(next-line)               | Move to next line
(shell-command <cm>)      | Execute shell command <cm>
(sleep-for <n>)           | Wait `n` seconds
(yank)                    | Yank text


--------------------------------------------------------------------------


Tools
-------

Thanks to Emacs Lisp, Emacs is very customizable and can be extended,
currently there are a lot of packages for Emacs, for almost anything
you need, here are some examples I have been trying:

**Elfeed**

Elfeed is a Package that allows you to read your feeds using Emacs.
You can find it in the package list (`package-list-packages`), Once
you have installed elfeed you can configure your favorite RSS or Atom
feeds, this can be done in your `.emacs` file (what a surprise):

```clojure
(setq elfeed-feeds
   '("http://hugo-dc.com/atom.xml"
     "http://emacsredux.com/atom.xml"
     ;; Add your favorite feeds here
	 ))
```

Once you have configured your `.emacs` file the position your cursor
in the last parantheses of the previous snippet, and press `C-x C-e`
this will evaluate it, this way `elfeed` will know where to look
at. Now you can start elfeed pressing `M-x` and running `elfeed`.

You will see an empty list, you can update your database pressing `g`,
this will start updating your feeds, and generates a list:

![](/images/2015090322111078.png)

You can navigate using `C-n`, `C-p`, and press enter to start reading
a blog post.

**eww**

Eww is a web browser embedded in Emacs, you can start it with the
`eww` command. Immediately it will ask you for an URL or for some
keywords, if you do not provide an URL then it will open
[duckduckgo.com](http://duckduckgo.com) and search for the keywords
you provided.

![](/images/2015090322163112.png)

Eww can even show images. It doesn't render a web page as Firefox or
Chrome would do, but it's good if you only want to browse a website
that contain mostly text.

**Twittering**

Twittering is an emacs package that allows you to view your twitter
feed directly in Emacs. After you install this package you can run it
using `twit`, that command will ask you if you want to authorize the
app, it will open a twitter page where you can authorize it, the page
generates a PIN, which you have to insert in Emacs minibuffer (using
`C-y`), after that you will see your twitter feed:

You can press **`i`** to show your contacts profile pictures. You can
update your status pressing **`u`**.

Here is how it looks:

![](/images/2015090322281211.png)


--------------------------------------------------------------------------


Resources
---------

[10 tips for Emacs on Windows](http://gregorygrubbs.com/emacs/10-tips-emacs-windows/)

[Emacs Rocks](http://emacsrocks.com/)

[Docview on Windows](http://emacsworld.blogspot.mx/2009/08/getting-docview-to-work-on-windows.html)

[Emacs Wiki](http://emacswiki.org/emacs/EmacsWiki)

[Brave Clojure - Starting using Emacs](http://www.braveclojure.com/basic-emacs/)


