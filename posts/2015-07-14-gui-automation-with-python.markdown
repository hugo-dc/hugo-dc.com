---
title: GUI Automation With Python
---

Some of my late requirements needed to do a lot of steps manually, a set of
repetitive actions to be done. 

But we are developers, we automate repetitive stuff. You can create a program
in one hour or two, and avoid 10 hours of manual labor. 

![](/images/comic.jpg)

We are always creating scripts (Python, Bash, Ruby, etc) to automate our tasks
and make our life easier. But usually the stuff we automate are the ones that
we can run in a console, but sometimes we need to automate the interaction with
a GUI.

In this post I will show you some Python modules that can be very helpful when
you need to automate GUI interaction.

-------------------------------------------------------------

Mouse Automation [Autopy]
=========================

The python module I use for mouse automation is [Autopy](https://github.com/msanders/autopy/), 

You can install this module with:

```
easy_install autopy
```

Here are the methods I usually use when I work with autopy:

Moving the mouse pointer:

```python
import autopy

autopy.mouse.move(x,y)
```

Your mouse pointer will _teleport_ to the **x** and **y** position.

I say _teleport_ because the pointer gets immediately to the **x** and **y** position,
you won't see the pointer moving. If you want to see your pointer moving from
a start position to a **x/y** position then you could use the `smooth_move` method.

```python
import autopy 

autopy.mouse.smooth_move(x,y)
```

You see the mouse pointer moving (veeeery slow) in your screen until it gets to
the **x** and **y** position.

If you really need your pointer to move from one point to another (i.e. If you
want to select something), you can do that, moving point by point until you
reach the final point:

```python
def select(ax, ay, bx, by):
    # Move to the start position
    move(ax,ay)

    # Click in the start position 
    autopy.mouse.toggle(True)

    # Wait 0.8 seconds before moving 
    time.sleep(0.8)
    print "Moving..."
    x = ax 
    y = ay 

    # Increment x and y by one
    # This will move to the next position 
    # Stops when it gets the desired position 
    while x < bx or y < by: 
        if x < bx: x += 1 
        if y < by: y += 1
        autopy.mouse.move(x,y)

    # Release the mouse button
    autopy.mouse.toggle(False)
```

In the example above, we see another method `autopy.mouse.toggle`, this method
allows to click in some point and hold it until you want to release it.

-------------------------------------------------------------

Keyboard Automation (Windows) [win32com] 
==============================

You can send keys as if it comes from a keyboard using win32com. Here is an
example:

```python 
import win32com.client as client

shell = client.Dispatch("WScript.Shell")

shell.sendKeys(key)
```

In [this site: http://ss64.com/vb/sendkeys.html](http://ss64.com/vb/sendkeys.html) you can find how to send
special keys such as INSERT, ENTER, etc.

-------------------------------------------------------------

Other resources
===============

Check Current Window (Windows) [win32gui] 
--------------------------------

Using the mouse automation you can operate GUI programs, in my case when
I clicked a button in my program I had to wait until the program returns
a result, when the program returned a result a little pop up appeared on the
screen, so I needed to figure out how my script could know when that little pop
up is visible.

To do this I used module win32gui:

```python
title = win32gui.GetWindowText(win32gui.GetForegroundWindow())

if title == POPUP_TITLE:
    # continue_processing...
```

Clipboard [Tk]
--------------

Once the popup is in my screen I can copy the result (in my case the number of
records found in a SAP table), y selected the result using mouse methods from
**autopy**, I send CTRL + C keys using **win32com**. And I imported the value
from the Clipboard to my script using Tk:

```python
from Tkinter import Tk

tk = Tk()

clipboard = tk.clipboard_get()

if int(clipboard) == 0:
    # If no record found do something
```

And that's it. I hope you like this post, if you have a comment or question
please let me know (You can find my twitter account at the top of the page). 


