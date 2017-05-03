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

TODO

I'm using the same Emacs configuration in my Windows box (Thanks to Dropbox), so
I needed something like `gnome-screenshot` to take screenshot, but I didn't wanted to
install anything there, then I decided to create a simple screenshot tool.

I found
[a code example](https://www.codeproject.com/Articles/485883/Create-your-own-Snipping-Tool)
that I could extend to make something similar to scrot. The
[LICENSE](https://www.codeproject.com/info/cpol10.aspx) allows me to do that.

So I changed the program to behave like `gnome-screenshot`, the program name in windows is
`org-shot`. This program receives the picture path as parameter.

Here is the Emacs Lisp Functions I created to take screenshots in Windows and
GNU/Linux and show the pictures in Org Mode.

* Get screenshot program, depending on the operating system we are using, returns the correct program
name:

```elisp
(defun get-screenshot-program ()
  (if (eq system-type 'windows-nt)
      "c:/users/admin/Dropbox/Org/org-shot.exe"
    "gnome-screenshot -a -f"))
```

* Get base dir, indicates the path where the images will be stored.

```elisp
(defun get-base-dir ()
  (if (eq system-type 'windows-nt)
      "C:/Users/ADMIN/Dropbox/Org/images/" ;; Windows
    "~/Dropbox/Org/images/"))              ;; GNU/Linux
```

* Screencap.

```elisp
(defun screencap ()
  "Captures an image and generates image name (Works on Windows/Linux)"
  (interactive)
  (let ((base-dir  (get-base-dir))
	(image-dir (format-time-string "%Y%m"))
	(image-name (format-time-string "%d%H%M%s")))
    (progn
      (when (eq (cdr (assoc 'fullscreen (frame-parameters))) 'maximized)
    	(suspend-frame))
      (when (not (file-exists-p (concat base-dir image-dir)))
    	(make-directory (concat base-dir image-dir)))
      (shell-command (concat (get-screenshot-program) " " base-dir image-dir "/" image-name ".png"))
      (insert "[[")
      (insert (concat "~/Dropbox/Org/images/" image-dir "/" image-name ".png"))
      (insert "]]")
      (org-redisplay-inline-images))))
```

Here is the explanation:

Gets the base directory, depending on the operating system.

```elisp
  (let ((base-dir  (get-base-dir))
```
Gets the image subdirectory, this is YYYYMM, for example 201704. I did this
because I created a lot of files in the `images/` directory, so it's much better
to group them by month.

```elisp
	(image-dir (format-time-string "%Y%m"))
```	

Define the image name, the image name will be the day number + timestamp

```elisp
	(image-name (format-time-string "%d%H%M%s")))
```

If Emacs is on top of all other application, this will minimize Emacs allowing
you to take the screenshot.

```elisp
      (when (eq (cdr (assoc 'fullscreen (frame-parameters))) 'maximized)
    	(suspend-frame))
```

If Emacs does not covers all screen, then it will remains as it is and you can
take a screenshot even at the Emacs window.

If the directory YYYYMM does not exists, emacs will create it:

```elisp
      (when (not (file-exists-p (concat base-dir image-dir)))
    	(make-directory (concat base-dir image-dir)))
```

After that we call a shell command to execute our screenshot program (`scrot` or
`org-shot`):

```elisp
      (shell-command (concat (get-screenshot-program) " " base-dir image-dir "/" image-name ".png"))
```

When the screenshot is taken insert the image path using Org format.

```elisp
      (insert "[[")
      (insert (concat "~/Dropbox/Org/images/" image-dir "/" image-name ".png"))
      (insert "]]")
```

We call `org-redisplay-inline-images` at the end in order to show the previous
code as an image/picture.

I hope this post may help you to integrate screenshots to your Org files, or if
you are still not using Emacs I hope you want to try it. In any case, if you
have any comment or question please [let me know](http://twitter.com/hugo_dc).
