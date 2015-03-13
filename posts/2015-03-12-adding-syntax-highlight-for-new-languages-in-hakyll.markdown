---
title: Adding Syntax Highlight for New Languages in Hakyll
---

I decided to start blogging again, I used to have a blog before, my first blog
was made in blogspot, there I used to write about ABAP, Python, Linux. Later,
I got my own domain and intalled Wordpress, I created many blog posts using
Wordpress, but I didn't like it, there was a lot of administration to be done,
a lot of spam comments, I needed something simple, then I decided to use
a static blog generator, I tried Jakyll, but again, I didn't like it. Then
I started to learn a little bit of Haskell and while I was reading some blog
posts about this language, I realized that some of them used Hakyll as engine.
Then I decided to give it a try. I found Hakyll very easy to use and to
customize.

I'm planning to write about ABAP, the programming language I used every day,
Python, the programming language I use often for creating script to automate
daily tasks and side-projects, and Maybe Haskell.

I already "ported" some old Wordpress posts to markdown, those posts are
related to ABAP, ABAP is a programming language used in an ERP called [SAP](http://sap.com), it's
number 16 on [Tiobe list for March 2015](http://www.tiobe.com/index.php/content/paperinfo/tpci/index.html). 
But its syntax is not highlighted automatically by Hakyll:

![](/images/hakyll.jpg)

Generated HTML:

```html
<pre class="abap"><code>PROCESS BEFORE OUTPUT.
* MODULE STATUS_0100.
  MODULE show_multiline.</code></pre>
<ul>
```

When you install Hakyll a lot of required libraries are installed with it,
including a library called `highlighting-kate`.

This library handles the syntax highglight of many different programming
languages. 

We can see the source code of library `highlighting-kate` this way, run:

```
pandoc --version
```

This will show you the version of `highlighting-kate` that was used to compile
`pandoc`. Rund command:

```
cabal unpack highlighting-kate-0.5.11.1 
```

This will create a new directory containing the source code.

In order to allow Hakyll to detect the ABAP syntax
correctly I have to rebuild the `highlighting-kate` library, but this time
including the `xml` file that corresponds to ABAP.

I found the `abap.xml` file in a [GitHub repository](https://github.com/PonyEdit/PonyEdit/blob/master/syntaxdefs/abap.xml).

Saved the `abap.xml` file into `highlighting-kate-0.5.11.1/xml/abap.xml`.

This `abap.xml` will be used to generate a Parser for this language.

To generate  the parsers run:

```
runhaskell ParseSyntaxFiles xml
```

This will generate a Haskell source code for every program in the `xml` directory.

And installed the library again, in the highlighting-kate repository
I executed:

```shell
λ cabal install
```

This command will reinstall the library.

Now, you have to recompile your `site.hs` source code, to generate a new
executable that uses our new compiled library. Then you can build your blog
again running command: `site rebuild`.

Now the source code for your new programming language should be highlighted correctly:

```abap
PROCESS AFTER OUTPUT.
* MODULE STATUS_0100.
  MODULE show_multiline.
```


