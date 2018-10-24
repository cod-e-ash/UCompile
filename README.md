# RPG Find Loop/Condition's Start and End

This is part of my **`RPG Utils`** series to help overcome some of the day-to-day activities which can be automated.  
During analysis of old rpg or cl codes, we come across sections where its very confusing that which line is part of which if condition or dow loop. This happens moslty when we deal with old rpg codes or some linear code with no indentation. This ustility will help to find the line to start of conditions like (IF, SELECT etc.) and also looping conditions like (DOW, DOU, WHILE, FOR etc.)

## Getting Started

These instructions will get you a copy of the project up and running on your local machine for development and testing purposes.

### Prerequisites

You need to have AS400 Machine access (duh!)  
Create a RPGUtils Source File to store the files.
```
CRTSRCPF RPGUTILS
```

### Program and Object Descriptions  
  
  * `IFLBLF`  
  This is a PF source which will hold output after running the program on any give source code file.  

  * `IFLBLC`  
  Driver CL program.  

  * `IFLBLCMD`  
  Driver command source file.  
  
  * `IFLBLP`  
  Main program.  


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
  IFLBLC      CLLE    
  IFLBLCMD    CMD     
  IFLBLF      PF      
  IFLBLP      SQLRPGLE
```
**Step3.**
Use below command to compile.
```   
CRTPF FILE(YOURLIB/IFLBLF) SRCFILE(YOURLIB/RPGUTILS) SRCMBR(IFLBLF)           

CRTSQLRPGI OBJ(YOURLIB/IFLBLP) SRCFILE((YOURLIB/RPGUTILS) SRCMBR(IFLBLP) OBJTYPE(*PGM) REPLACE(*YES)  

CRTBNDCL PGM(YOURLIB/IFLBLC) SRCFILE((YOURLIB/RPGUTILS) SRCMBR(IFLBLC) REPLACE(*NO)               

CRTCMD CMD(YOURLIB/IFLBL) PGM(*LIBL/IFLBLC) SRCFILE(YOURLIB/RPGUTILS) SRCMBR(IFLBLCMD) REPLACE(*YES)  
```


## Running

```
IFLBL <source member name> <source file> <source library>
```
E.g.
IFLBL SRCMBR(IFLBLP) SRCFIL(RPGUTILS) SRCLIB(YOURLIB)
This will output a file IFLBLF. FSTRSEQ is the start line, FEBDSEQ is the end of the sequence.
Rest fields will tell you if there are else conditions etc.
```
SELECT * FROM IFLBLF

O/P:
FID    FSTRSEQ    FENDSEQ   FIFDATA                                              
  1      47.00     220.00           DoW SQLCOD = 0;                              
  2      50.00     216.00             If %SubSt(SRCDTA:7:1) <> '*' And (         
  3      55.00     140.00               Select;                                  
  4      69.00      72.00                 If %SubSt(wkStrUpper:26:5) = '  CAS' Or
  5     101.00     113.00                 If FElseSq1 = *Zeros;                  
  6     128.00     130.00                 If wkCasFlg = *On;                     
  7     146.00     150.00               If %Scan('//':SRCDTA) > 0;               
  8     155.10     155.13                 If wkFoundStart <= 0;                  
  9     155.16     155.22                 If wkFoundStart > 0;                   
```

## Authors

Ashish Bagaddeo

## License

This project is licensed under the Apache License v2.0 - see the [LICENSE.md](LICENSE.md) file for details

## Acknowledgments
Thanks www.PUB400.com for hosting a public server.
