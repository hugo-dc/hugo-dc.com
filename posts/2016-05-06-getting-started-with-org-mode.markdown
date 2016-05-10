---
title: Getting Started With Org Mode
---

This week I've been playing with Org Mode. Org Mode is an Emacs
package that allows you to take notes, maintain TODO lists, and get
organized.

There are a lot of resources to learn how to use Org Mode, a lot of
tutorials, screencast, big manuals, but here I will write the minimum
you need to know (All I know about Org) to get started with Org Mode.

# Org File

The first thing you need to start your new organized life is to have a
`.org` file.

In my case I have two Org Files:

- personal.org
- work.org

When you open an Org file, Emacs automatically loads Org Mode:

![](/images/orgmode.png)

# Outlining

In my `personal.org` file, I have organized different topics, for
example:

![](/images/outline1.png)

Inside `Programming` I can organize it by Programming Language:

![](/images/outline2.png)

Notice that I added two stars to those topics, and Org Mode sets a
different color. Here, the programming languages topics are inside of
the main Programming Topic.

Now, If I go to the beginning of the Main Topic (Programming) and
press `TAB`, I can hide the subtopics, this way I can focus on the topic
I want, and hide the others.

![](/images/main.gif)

The three dots at the end means there is content inside.

Inside you can type any text, links (to websites and files).

If you want to insert a link press `C-c C-l`:

![](/images/link.png)

Press `TAB` to see the possible options:

![](/images/link-options.png)


I selected `file:`, and pressed enter, then you set the filepath
(`~/.emacs.d/init.el`), a Description (`my init.el`) and the link is
created:

![](/images/link-2.png)

Click on the link and your file or website will be opened.

If you press backspace you can see the `text` representation of the
link:

![](/images/link-text.png)

The link consists on:

```
[[PATH][DESCRIPTION]]
```

When you press backspace you actually remove the last `]`.


# TODOs and Checklists

Now, let's work with TODO lists. Imagine I want to Study Haskell, I
can create a TODO item, to remember to study:

![](/images/haskell.png)

But what topics I want to learn about Haskell?, I can create a
Checklist with each item:

![](/images/haskell-topics.gif)

When I finish adding the first item, I just press `M-S-RET`, and the
new checklist item is ready to be created.

If I move to a checklist item and press `C-c C-c` the item is checked,
press `C-c C-c` again, and the item is unchecked again.

You can know which percentage or how many tasks are already finished
using:

- `[%]` - For percentage indicator
- `[/]` - For total tasks finished indicator

![](/images/indicators.gif)

Cool right?

# Agenda

OK, but what happens if I write a lot of TODO's and they get buried in
different topics or subtopics?.

For this, Org has an Agenda, the Agenda show all TODO items existing in
your file.

First you need to tell Emacs that you want the Agenda to manage your
org file, you do this with `C-c [`, this will show you this message:

![](/images/file-added.png)

Then you can call the agenda executing `M-x` -> `org-agenda`, or you can
add a shortcut (as I did) in your `init.el`, then you can press `C-ca`
and the agenda will start:

```elisp
(global-set-key "\C-ca" 'org-agenda)
```

Remember, if you add it to your `init.el` it will take effect when you
start Emacs again, you can position at the end of the line and press
`C-x C-e` to load the shortcut right away.

The command shows you this menu:

![](/images/agenda.png)


If you press `t` it will show you ALL the TODO items for your org
files (all your all files that are in the agenda file list)

![](/images/agenda-todo.png)

You can navigate the Agenda using `n` (Next Line) and `p` (Previous
Line).

If the cursor is located in a TODO item and you press `TAB`, the
agenda takes you to the exact file and line where your TODO item is
located.

This is good, I can see ALL my TODO items, but what if I have a lot of
items, but today I just want to work with three, which are the most
important/urgent?.

You can schedule TODO items using `org-schedule`, again, it's better
to create a shortcut for this:

```elisp
(global-set-key "\C-cs" 'org-schedule)
```

When you press the shortcut the agenda shows you a calendar, there you
can click on the day you want to schedule that task, or press enter if
you want to schedule it for today:

![](/images/schedule.png)

The TODO item shows the SCHEDULE date:

![](/images/sched-tag.png)

You can see your scheduled items for the current Week/Date opening the
Agenda:

`C-c a`

![](/images/agenda.png)

And pressing `a` again, then you see the Agenda for the current Week:

![](/images/agenda-week.png)

If you press `d` the Agenda only shows the current day data, you can
go back to Week view pressing `w`:

![](/images/agenda-day.png)

You can also add Deadlines to your items using `org-deadline`, or
again, adding a shortcut:

```elisp
(global-set-key "\C-cd" 'org-deadline)
```

![](/images/deadline.png)

The deadline is also visible in the Agenda:

![](/images/agenda-deadline.png)

Finally, to close a Task you press `C-c C-t`:

![](/images/done.png)

You can tell Org to log the timestamp for closed task, you just need
to add this to your `init.el`:

```elisp
(setq org-log-done 'time)
```

![](/images/closed.png)


I hope this little article can help you to get started with Org.

If you have a comment or question please let me know, [I'm on twitter](http://twitter.com/hugo_dc)!
