---
title: Useful string functions in ABAP
---

Replace
-------

You can remove a character from a string using the sentence REPLACE, you just need to replace the character for an empty character.
Here is an example:
ABAP

```abap
    REPORT zreplace.
 
    PARAMETERS:
        p_data TYPE c LENGTH 20.
 
    REPLACE ALL OCCURRENCES OF '|' IN p_data WITH ''.
 
    WRITE:/ p_data.
```

This program asks for an input parameter, and will replace every ‘|’ character for an empty character as you can see in the following images:

![replace](/images/replace.png)

All the occurrences of the `|` character will be replaced by an empty character.

Translate
---------

You can use the sentence TRANSLATE to convert a string to uppercase or lowercase.

```abap
REPORT ztranslate.
 
DATA:
      my_string TYPE string VALUE 'Hello world'.
 
WRITE:/ my_string.
 
TRANSLATE my_string TO UPPER CASE.
WRITE:/ my_string.
 
TRANSLATE my_string TO LOWER CASE.
WRITE:/ my_string.
```

If you execute that program you sould get the following result:

![translate](/images/translate.png)

There is another way you can use the sentence TRANSLATE. You can replace a character by another:

```abap
REPORT ztranslate_pattern.
 
DATA:
      my_string TYPE string VALUE 'Hello world'.
 
TRANSLATE my_string USING 'oaeo'.
 
WRITE:/ my_string.
```

And the result is:

![Translate pattern](/images/translate_pattern.png)

The sentence TRANSLATE changed the value of `my_string` based on a pattern, to understand how that pattern work is very simple, the first character is replaced by the second, the third is replaced by the fourth and so on.

Reverse string 
-----------------

You can reverse the content of a variable type C with fixed length using the Function Module `STRING_REVERSE`.
You can use that FM as in the following example:

```abap 
REPORT zreverse.
 
DATA:
      my_var TYPE c LENGTH 100.
 
START-OF-SELECTION.
 
  my_var = 'hello world'.
 
  CALL FUNCTION 'STRING_REVERSE'
    EXPORTING
      string    = my_var
      lang      = sy-langu
    IMPORTING
      rstring   = my_var
    EXCEPTIONS
      too_small = 1
      OTHERS    = 2.
 
  IF sy-subrc = 0.
    WRITE:/ my_var.
  ENDIF.
```
 
You must be very careful with this FM, because will not work if you use it with a variable type STRING, if you use it with a variable type STRING you will receive a short dump  ` OBJECT_NOT_CHAR`.

Scape single quote in string variable 
--------------------------------------

There are two ways you can scape the single quote character inside of a string definition.

```abap
REPORT zquote.
 
DATA:
      my_var TYPE string.
 
my_var = `SELECT * FROM ZTEST WHERE A = '5'.`.
 
WRITE:/ my_var.
 
my_var = 'SELECT * FROM ZTEST WHERE A = ''5''.'.
 
WRITE:/ my_var.
```

In the first definition we are using a more readable way to assign a value to a string, instead of using single quotes to start the definition we use backquote.
In the second definition we are using double single quote character to represent a single quote character inside the string definition.
 



