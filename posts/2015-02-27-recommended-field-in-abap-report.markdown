---
title: Recommended field in ABAP Report
---

When we create Modul Pool programs we can set the property
_Recommended_ to a field, the field displays a symbol and the
user knows that it is an "obligatory" field, but if the user
let the field empty, the program will work because it is not an
obligatory field, just recommended.

In ABAP reports we can create that behaviour too, but it takes
a little of ABAP coding. Here is how to do it in an ABAP report:


```abap
REPORT zexample.

PARAMETERS: p_ebeln LIKE ekko-ebeln,
            p_ebel2 LIKE ekko-ebeln.

AT SELECTION-SCREEN OUTPUT.
    LOOP AT screen.
        IF screen-name = 'P_EBELN'.
            screen-required = 2.
            MODIFY screen.
        ELSEIF screen-name = 'P_EBEL2'.
            screen-required = 1.
            MODIFY screen.
        ENDIF.
    ENDLOOP.

START-OF-SELECTION.
    WRITE: 'Recommended field in ABAP Report'.

```

This report contains 2 parameters, and both seems to be obligatory:

![](/images/recommended.jpg)

But, if you leave the first field blank and hit F8, program
will be executed and the final output is reached:

![](/images/recommended2.jpg)

![](/images/recommended3.jpg)
