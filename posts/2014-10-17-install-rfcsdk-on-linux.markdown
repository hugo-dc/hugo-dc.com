---
title: Install RFCSDK on Linux
---

This SDK is only available on the SAP Market Place.
SDKs are available in the category called Additional Components. You are going 
to get a SAR file, this is a compressed file, to extract the files contained in 
this file you will need a tool called SAPCAR, that again is available at the 
SAP Market Place in the same category.

Now that you have both, SAPCAR and the SAR file, you should run the following 
command to extract your files:

```bash
    SAPCAR.EXE -xvf RFC_SDK1.4.SAR
```

Note that your SAPCAR and RFCSDK.SAR file may be different depending on the version 
that you are downloading.

Running that command will generate a new folder called rfcsdk. 
The easy way to put that libraries in your system is to copy the contents of rfcsdk/lib 
to your /usr/lib path, and rfcsdk/include to /lib/include, obviously you must be root 
to do that.

```bash
    sudo cp rfcsdk/lib/* /usr/lib/
    sudo cp rfcsdk/include/* /usr/include/
```

Now you have that libraries, needed by many programs and programming languages like 
PHP, Python, Ruby, in order to have a communication with the SAP System.
