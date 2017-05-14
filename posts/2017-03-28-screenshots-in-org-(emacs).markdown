---
title: Screenshots in Org (Emacs)
---

This is my third post about Emacs and again is about Emacs Org. Something I
really liked about Emacs is that you can view pictures inside buffers. This is
an amazing feature.

At the beginning I used to copy an image to a subfolder called `images` then I
manually set the path to the image in my Org file:

```
[[~/Org/images/mypicture.png]]
```

And then `M-x org-redisplay-inline-images`, and it was great to look at the
picture inside my Org file. But after doing this a few times I knew I could do it
better.

Currently I'm using the same Emacs configuration in Windows and in GNU/Linux
(Thanks to Dropbox). In Arch(linux) I could call `gnome-screenshot` to take
screenshot, but I don't know something like it for Windows and I didn't wanted
to install anything new or complicated, then I decided to create a simple
screenshot tool.

I found
[a code example](https://www.codeproject.com/Articles/485883/Create-your-own-Snipping-Tool)
that I could extend to make something similar to `gnome-screenshots`. The
[LICENSE](https://www.codeproject.com/info/cpol10.aspx) allows me to do that.

So I changed the program to behave like `gnome-screenshot`, the program name in windows is
`org-shot`. This program receives the picture path as parameter. So you could
call it from the Windows terminal as:

```
org-shot images/test.png
```

And that would allow you to select a screen region and the resulting picture
will be saved as `test.png` in the `images` directory.

You can download `org-shot` binary from the [GitLab repository](https://gitlab.com/hugo_dc/org-shot/tree/master/BNScreenshot/bin/Debug), but I highly recommend you recompile it yourself.

So now I have the programs I need for selecting a screen region and storing the
picture in a file, `gnome-screenshots` in Archlinux and `org-shot` in
Windows. Then we just need the Emacs Lisp code to generate the image name,
calculate the path and call the screenshot program and save the picture.

Here is the Emacs Lisp code I wrote to make it work:

* *get-sreenshot-program*: First, we need to know what screenshot program we need, depending on the
operating system we are using, the function returns the correct program
name:

```lisp
(defun get-screenshot-program ()
  (if (eq system-type 'windows-nt)
      "c:/users/admin/Dropbox/Org/org-shot.exe" ;; for Windows
    "gnome-screenshot -a -f"))                  ;; for GNU/Linux
```

* *get-base-dir*: Get base dir, indicates the path where the image files will be
  stored, in Windows I'm using the full path:

```lisp
(defun get-base-dir ()
  (if (eq system-type 'windows-nt)
      "C:/Users/ADMIN/Dropbox/Org/images/" ;; Windows
    "~/Dropbox/Org/images/"))              ;; GNU/Linux
```

* *screencap*: This is our main function

```lisp
(defun screencap ()
  "Captures an image and generates image name (Works on Windows/Linux)"
  (interactive)
  (let ((base-dir  (get-base-dir))                                       ;; (1)
    (image-dir (format-time-string "%Y%m"))                              ;; (2)
    (image-name (format-time-string "%d%H%M%s")))                        ;; (3)
(progn
      (when (eq (cdr (assoc 'fullscreen (frame-parameters))) 'maximized) ;; (4)
    	(suspend-frame))                                                 ;; (5)
      (when (not (file-exists-p (concat base-dir image-dir)))            ;; (6)
    	(make-directory (concat base-dir image-dir)))                    ;; (7)
      (shell-command (concat (get-screenshot-program) " " base-dir image-dir "/" image-name ".png"));;(8)
      (insert "[[")                                                          ;; (9)
      (insert (concat "~/Dropbox/Org/images/" image-dir "/" image-name ".png"))
      (insert "]]")
      (org-redisplay-inline-images))))  ;; (10)
```

Here is how it works:

(1) Gets the base directory, depending on the operating system and stores it in `base-dir`

```lisp
  (let ((base-dir  (get-base-dir))
```

(2) Gets the image subdirectory, this is YYYYMM, for example 201705. I did this
because I created a lot of files in the `images/` directory at first, so it's much better
to group them by month.

```lisp
	(image-dir (format-time-string "%Y%m"))
```	

(3) Define the image name, the image name will be the day number + timestamp

```lisp
	(image-name (format-time-string "%d%H%M%s")))
```

(4,5) If Emacs is on top of all other application (maximized), this will minimize Emacs allowing
you to take the screenshot.

```lisp
      (when (eq (cdr (assoc 'fullscreen (frame-parameters))) 'maximized)
    	(suspend-frame))
```

If Emacs does not covers all screen, then it will remains as it is and you can
take a screenshot even including the Emacs window.

(6, 7) If the directory YYYYMM does not exists, emacs will create it:

```lisp
      (when (not (file-exists-p (concat base-dir image-dir)))
    	(make-directory (concat base-dir image-dir)))
```

(8) After that we call a shell command to execute our screenshot program (`gnome-screenshots` or
`org-shot`):

```lisp
      (shell-command (concat (get-screenshot-program) " " base-dir image-dir "/" image-name ".png"))
```

(9) When the screenshot is taken insert the image path using Org format.

```lisp
      (insert "[[")
      (insert (concat "~/Dropbox/Org/images/" image-dir "/" image-name ".png"))
      (insert "]]")
```

This will add a string in the current Org buffer similar to this:

```
[[~/Dropbox/Org/images/201705/10205533224455.png]]

```

And Org will represent it as a link (only showing the image path)

(10) We call `org-redisplay-inline-images` at the end in order to show the previous
code as an image/picture, not as a link.

And that's it. Now we can see the picture in our Org buffer:

![](/images/screenshot.png)

It is very possible that you don't need all the code (maybe you just use
GNU/Linux, or just Windows, or another OS), but I think you can use the code and
adapt it to your needs, in any case, I hope this post may help you to integrate
screenshots to your Org files, or if you are still not using Emacs I hope you
want to try it. If you have any comment, question or ideas on how
to improve the code, please [let me know](http://twitter.com/hugo_dc).


