# Compile with Source and defaults

<em>This is part of my **`RPG Utils`** series to help overcome some of the day-to-day activities which can be automated.</em>  

We usually compile our objects in source while testing. Also, there are some parameters that we need to change everytime before compilation. This program will help you with that.   


## Getting Started

These instructions will get you a copy of the project up and running on your local machine for development and testing purposes.

### Prerequisites

You need to have AS400 Machine access (duh!)  
Create a RPGUtils Source File to store the files.
```
CRTSRCPF RPGUTILS
```

### Program and Object Descriptions  
  
  * `UCOMPILE`  
  This is the main program.  

  * `UCOMPCMD`  
  Driver command source file.  


### Installing

**Step 1.**
Upload all files to AS400 server, use ftp. <em>DO NOT CHANGE THE MODE TO BINARY</em>.
```
  open pub400.com
  username
  password
  cd /QSYS.LIB
  cd YOURLIB.LIB
  cd RPGUTILS.FILE
  mput *.MBR
  disconnect
  quit
```
**Step 2.**
Change the atribute type accordingly once uploaded.
```
  UCOMPILE    CLLE    
  UCOMPCMD    CMD     
```
**Step3.**
Use below command to compile.
```   
CRTBNDCL PGM(YOURLIB/UCOMPILE) SRCFILE((YOURLIB/RPGUTILS) SRCMBR(UCOMPILE) REPLACE(*NO)               

CRTCMD CMD(YOURLIB/UCOMPILE) PGM(*LIBL/UCOMPILE) SRCFILE(YOURLIB/RPGUTILS) SRCMBR(UCOMPCMD) REPLACE(*YES)  
```
**Step4.**
Add new user option.  

Enter command `WKRMBRPDM` and then goto user options using `F16`.  
Press `F6` to add  a new user option. 
Set `Option` as `CP`.  (Instead of CP, you can use any aything that you want for option.)  
Set `Command` as `UCOMPILE(&L &F &N &S)`  


## Running
Now, instead of compiling with Option 14, you can use CP to compile with your options.

## Authors

Ashish Bagaddeo

## License

This project is licensed under the Apache License v2.0 - see the [LICENSE.md](LICENSE.md) file for details

## Acknowledgments
Thanks www.PUB400.com for hosting a public server.
