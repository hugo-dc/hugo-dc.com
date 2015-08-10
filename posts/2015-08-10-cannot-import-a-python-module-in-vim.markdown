---
title: Error importing a Python module in Vim
---

I wanted to test a Vim plugin I created a long time before, but I was receiving
an error:

![](/images/traceback.jpg)

But when I imported the module in the command line everything went fine, so
I supposed that the Python imterpreter used by Vim used a different path to
read the Python modules.

To know the path that PythonVim is using I executed the following function in
Vim:

```viml

:python import sys
:python print sys.path

```

Vim printed a Python list with each path considered by the interpreter,
I realized that the `site-packages` path was not listed there. So, in my
plugin, I created a new Vim function to add the `site-packages` directory to
the current path:

```python

import sys
for path in sys.path:
    if path[-3:] == 'Lib':
        sys.path.append(path + '\\site-packages')
        break
```

Then in the main function of my plugin I checked if the import of my library
was successfull, if not then I call the Vim function `A4V_addpath()`:

```python

import vim
try:
    import easysap
except Exception, e:
    vim.eval('A4V_addpath()') 

```


