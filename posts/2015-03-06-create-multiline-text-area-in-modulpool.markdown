---
title: Create multiline text area in ModulPool
---

Sometimes we need to addd a `Text Area` in our ModulPool. Here is how it can be
done:

* Create a ModulPool program.

* Create a new screen (dynpro).

* In our screen instert a Custom Control:

![](/images/cc_01.jpg)

* Set a name to your Custom Contol:

![](/images/cc_02.jpg)

* Save your screen, return to flow logic (quit screen painter), and activate.

* In PBO code, create a new module `SHOW_MULTILINE`:

```abap
PROCESS BEFORE OUTPUT.
* MODULE STATUS_0100.
  MODULE show_multiline.
```

* Double click on `show_multiline`, this will ask if you want to create the module:

![](/images/cc_03.jpg)

* Click on Yes, now you have to select where you want to create the new module,

  in my case I'm going to select my Main program:

![](/images/cc_04.jpg)

* You'll need to save changes made to your screen, click on yes:

![](/images/cc_05.jpg)

* Now, in our editor we have the new Module, something like this:

```abap
*&---------------------------------------------------------------------*
*&      Module  show_multiline  OUTPUT
*&---------------------------------------------------------------------*
*       text
*----------------------------------------------------------------------*
MODULE show_multiline OUTPUT.

ENDMODULE.                 " show_multiline  OUTPUT
```

* In TOP include, declare a new variable to create an object of class
  `cl_gui_textedit` and a variable to create an oject of class
  `cl_gui_custom_container`:

```abap
*&---------------------------------------------------------------------*
*& Include ZTEST_MPTOP                                                 *
*&                                                                     *
*&---------------------------------------------------------------------*

PROGRAM  ZTEST_MP.

DATA: v_text  TYPE REF TO cl_gui_textedit,
      cc_text TYPE REF TO cl_gui_custom_container.
```

* Now, let's get back to our module, inside our `show_multiline` module create
  an object for our container, and create an object for our multiline text (I
  recomment you create objects using button `Pattern`), please note that in the
  first object creation we send `CC_TEXT` as a string, but in second object
  creation we send the `CC_TEXT` object, not its name:

```abap

*&---------------------------------------------------------------------*
*&      Module  show_multiline  OUTPUT
*&---------------------------------------------------------------------*
*       text
*----------------------------------------------------------------------*
MODULE show_multiline OUTPUT.

CREATE OBJECT CC_TEXT
  EXPORTING
    CONTAINER_NAME              = 'CC_TEXT'
  EXCEPTIONS
    CNTL_ERROR                  = 1
    CNTL_SYSTEM_ERROR           = 2
    CREATE_ERROR                = 3
    LIFETIME_ERROR              = 4
    LIFETIME_DYNPRO_DYNPRO_LINK = 5
    others                      = 6.

CREATE OBJECT V_TEXT
  EXPORTING
    PARENT                 = CC_TEXT
  EXCEPTIONS
    ERROR_CNTL_CREATE      = 1
    ERROR_CNTL_INIT        = 2
    ERROR_CNTL_LINK        = 3
    ERROR_DP_CREATE        = 4
    GUI_TYPE_NOT_SUPPORTED = 5
    others                 = 6.
    .
```
* Create a new Transaction for your program, and run it, you should see
  something like this:
![](/images/cc_06.jpg)




