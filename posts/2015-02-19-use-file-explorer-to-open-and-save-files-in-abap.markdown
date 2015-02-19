---
title: Use file explorer to open and save files in ABAP
---

Sometimes we need the user to select a file in our filesystem,
maybe because we want to read that file and perform some
operations, or if we have some data in the SAP system and we
want to create a new file.

Here is an example of how to do this in an ABAP report. First,
we have to create a parameter:

```abap
PARAMETERS: p_file(132) DEFAULT 'C:\MyFile.txt'.
```

Then we can add a matchcode, by clicking that matchcode the
user can explore the filesystem. Here is how we create the
matchcode for the corresponding parameter:

```abap
AT SELECTION-SCREEN ON VALUE-REQUEST FOR p_file.
CLEAR l_filecab.
CLEAR l_rccab.

CALL METHOD cl_gui_frontend_services=>file_open_dialog
    CHANGING: file_table = l_filecab
              l_rccab
    EXCEPTIONS: others = 01.

IF l_rccab = 1.
* Here the result of the file explorer is set to our parameter
    READ TABLE l_filecab INDEX 1 INTO p_file.
ENDIF.
```

We have to define variables `l_filecab` and `l_rccab`

```abap
DATA:
  l_filecab TYPE filetable,
  l_rccab   TYPE i.
```

![](/images/file_explorer.jpg)
