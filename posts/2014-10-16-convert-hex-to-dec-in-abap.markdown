---
title: Convert HEX to DEC in ABAP
---

If you declare a variable type X (Hexadecimal) and assign a value to it, you can move that value to another variable with type I (Integer), and the value will be converted to Integer automatically.

```abap
REPORT znumbers.

data:
my_int type i.

parameters:
my_hex type x.

my_int = my_hex.

write:/ 'HEX: ', my_hex, /'INT: ', my_int.
```

![Hex2Dec](/images/hex_dec1.png)

Result:


![Hex2Dec](/images/hex_dec2.png)



