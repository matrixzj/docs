---
title: awk
tags: [bash]
keywords: awk 
last_updated: Jul 1, 2021
summary: "awk tips"
sidebar: mydoc_sidebar
permalink: bash_awk.html
folder: bash
---

# awk
=====

## System Variables

### **FS**    
field separator. By default, its value is a single space. FS can also be set to any single character, or to a regular expression.

### **OFS**   
output field separator. By default awk OFS is a single space character.   


```bash
$ echo "matrix" | awk 'BEGIN{FS="tr"}{print $1}'
ma

$ echo "matrix" | awk 'BEGIN{FS="tr"}{print $1, $2}'
ma ix

$ echo "matrix" | awk 'BEGIN{FS="tr";OFS="\n"}{print $1, $2}'
ma
ix
```

### **RS**    
record separator. By default, its value is a newline.  

### **ORS**   
output record separator. By default, its value is a newline.   

```bash
$ echo "matrix" | awk 'BEGIN{RS="tr"}{if(NR==2)print $1}'
ix

$ echo "matrix" | awk 'BEGIN{RS="tr"}{print}' | sed -ne 'l'
ma$
ix$
$

$ echo "matrix" | awk 'BEGIN{RS="tr";ORS="|"}{print}' | sed -ne 'l'
ma|ix$
|$
```

### **NF**    
number of fields for the current input record.  

### **NR**    
number of records being processed or line number.     

```bash
$ echo "a|b|c" | awk 'BEGIN{FS="|"}{print NF}'
3

$ echo "a|b|c" | awk 'BEGIN{RS="|"}{print NR}'
1
2
3
```

### **FILENAME** 
the name of the file being read     

```bash
$ echo "a|b|c" | awk '{print FILENAME}'
-

$ cat /tmp/student
matrix  1
eli 2
kirk    3
haiyu   4

$ cat /tmp/score
1 60
2 70
4 100

$ awk '{print FILENAME}' /tmp/student /tmp/score
/tmp/student
/tmp/student
/tmp/student
/tmp/student
/tmp/score
/tmp/score
/tmp/score
```

### **FNR**   
Number of Records relative to the current input file    

```bash
$ awk '{print NR}' /tmp/student /tmp/score
1
2
3
4
5
6
7

$ awk '{print FNR}' /tmp/student /tmp/score
1
2
3
4
1
2
3
```

### **CONVFMT**   
number-to-string conversions. By default, the value is '%.6g' 

### **OFMT**      
string-to-number conversions. By default, the value is '%.6g'    

```bash
$ awk -v OFMT="%d" 'BEGIN{print str = (5.5 + 3.2)}'
8

$ echo 0.77767686 |  awk '{ print "" 0+$0 }' OFMT='%.1g'
0.777677

$ echo 0.77767686 |  awk '{ print "" 0+$0 }' CONVFMT='%.1g'
0.8
```

### **ENVIRON**
An array containing the values of the current environment.   

```bash
$ awk 'BEGIN{for(item in ENVIRON){print item, ":", ENVIRON[item]}}'
AWKPATH : .:/usr/share/awk
OLDPWD : /home/matrix
SELINUX_LEVEL_REQUESTED :
SELINUX_ROLE_REQUESTED :
LANG : en_US.UTF-8
PYENV_VIRTUALENV_INIT : 1
LC_ALL : en_US.UTF-8
HISTSIZE : 1000
XDG_RUNTIME_DIR : /run/user/1001
USER : matrix
HISTFILESIZE : 10000
_ : /usr/bin/awk
SELINUX_USE_CURRENT_RANGE :
TERM : screen-256color
HISTTIMEFORMAT : %d/%m/%y %T
SHELL : /bin/bash
PYENV_SHELL : bash
SSH_CONNECTION : 31.215.112.217 63239 10.0.0.4 22
XDG_SESSION_ID : 56574
LESSOPEN : ||/usr/bin/lesspipe.sh %s
PATH : /home/matrix/.pyenv/plugins/pyenv-virtualenv/shims:/home/matrix/.pyenv/shims:/home/matrix/.pyenv/shims:/home/matrix/.pyenv/bin:/usr/local/bin:/usr/bin:/usr/local/sbin:/usr/sbin:/usr/local/go/bin:/home/matrix/.local/bin:/home/matrix/bin
MAIL : /var/spool/mail/matrix
SSH_CLIENT : 31.215.112.217 63239 22
PYENV_ROOT : /home/matrix/.pyenv
HOSTNAME : matrix-oracle-instance-1
HOME : /home/matrix
PWD : /home/matrix/git/documentations
SSH_TTY : /dev/pts/1
HISTCONTROL : ignoredups
LC_CTYPE : UTF-8
LOGNAME : matrix
SHLVL : 1
LS_COLORS : rs=0:di=38;5;27:ln=38;5;51:mh=44;38;5;15:pi=40;38;5;11:so=38;5;13:do=38;5;5:bd=48;5;232;38;5;11:cd=48;5;232;38;5;3:or=48;5;232;38;5;9:mi=05;48;5;232;38;5;15:su=48;5;196;38;5;15:sg=48;5;11;38;5;16:ca=48;5;196;38;5;226:tw=48;5;10;38;5;16:ow=48;5;10;38;5;21:st=48;5;21;38;5;15:ex=38;5;34:*.tar=38;5;9:*.tgz=38;5;9:*.arc=38;5;9:*.arj=38;5;9:*.taz=38;5;9:*.lha=38;5;9:*.lz4=38;5;9:*.lzh=38;5;9:*.lzma=38;5;9:*.tlz=38;5;9:*.txz=38;5;9:*.tzo=38;5;9:*.t7z=38;5;9:*.zip=38;5;9:*.z=38;5;9:*.Z=38;5;9:*.dz=38;5;9:*.gz=38;5;9:*.lrz=38;5;9:*.lz=38;5;9:*.lzo=38;5;9:*.xz=38;5;9:*.bz2=38;5;9:*.bz=38;5;9:*.tbz=38;5;9:*.tbz2=38;5;9:*.tz=38;5;9:*.deb=38;5;9:*.rpm=38;5;9:*.jar=38;5;9:*.war=38;5;9:*.ear=38;5;9:*.sar=38;5;9:*.rar=38;5;9:*.alz=38;5;9:*.ace=38;5;9:*.zoo=38;5;9:*.cpio=38;5;9:*.7z=38;5;9:*.rz=38;5;9:*.cab=38;5;9:*.jpg=38;5;13:*.jpeg=38;5;13:*.gif=38;5;13:*.bmp=38;5;13:*.pbm=38;5;13:*.pgm=38;5;13:*.ppm=38;5;13:*.tga=38;5;13:*.xbm=38;5;13:*.xpm=38;5;13:*.tif=38;5;13:*.tiff=38;5;13:*.png=38;5;13:*.svg=38;5;13:*.svgz=38;5;13:*.mng=38;5;13:*.pcx=38;5;13:*.m
```

### **ARGC**   
The number of command line arguments
### **ARGV**        
Array of command line arguments
```bash
$ awk 'BEGIN{print ARGC}' n=1
2

$ awk 'BEGIN{print ARGC; for(i=0; i<ARGC; i++){printf("%d: %s\n", i, ARGV[i])}}' n=1
2
0: awk
1: n=1
```

## Functions
### Numeric Functions
#### **exp(x)**    
Return the exponential of x (e ^ x) or report an error if x is out of range.

#### **int(x)**    
Return the nearest integer to x.

#### **log(x)**    
Return the natural logarithm of x, if x is positive; otherwise, return NaN (“not a number”) on IEEE 754 systems.

#### **rand()**    
Return a random number. The values of rand() are uniformly distributed between zero and one. The value could be zero but is never one.

#### **sqrt(x)**   
Return the positive square root of x.

### String Functions
#### asort(source [, dest [, how ] ]) / asorti(source [, dest [, how ] ])   
Both functions return the number of elements in the array *source*. For *asort()*, awk sorts the values of *source* and replaces the indices of the sorted values of *source* with sequential integers starting with one. If the optional array *dest* is specified, then *source* is duplicated into *dest*. *dest* is then sorted, leaving the indices of *source* unchanged. If the *source* array contains subarrays as values, they will come last, after all scalar values. Subarrays are not recursively sorted. 
The *asorti()* function works similarly to *asort()*; however, the indices are sorted, instead of the values.   

```bash
$ cat /tmp/test
last|de
first|sac
middle|cul

$ awk '{split($0, entry, "|"); source[entry[1]]=entry[2]} END{count=asort(source, dest); for (i=1; i<=count; i++) {print dest[i]}}' test
cul
de
sac

$ awk '{split($0, entry, "|"); source[entry[1]]=entry[2]} END{count=asorti(source, dest); for (i=1; i<=count; i++) {print dest[i]}}' test
first
last
middle
```

#### sub(regexp, replacement [, target])
Search *target*, which is treated as a string, for the leftmost, longest substring matched by the regular expression *regexp*. Modify the entire string by replacing the matched text with *replacement*. The modified string becomes the new value of *target*. Return the number of substitutions made (zero/failure or one/success).
The *regexp* argument may be either a regexp constant (/…/) or a string constant ("…"). In the latter case, the string is treated as a regexp to be matched. 
This function is peculiar because *target* is not simply used to compute a value, and not just any expression will do—it must be a variable, field, or array element so that sub() can store a modified value there. If this argument is omitted, then the default is to use and alter *$0*.

```bash
$ awk 'BEGIN{test = "matrix"; result=sub("tri", "", test); print test; print result}'
max
1

$ awk 'BEGIN{test = "matrix"; result=sub("tria", "", test); print test; print result}'
matrix
0
```

If the special character *&* appears in replacement, it stands for the precise substring that was matched by *regexp*. If the *regexp* can match more than one string, then this precise substring may vary.

```bash
$ awk 'BEGIN{test = "matrix"; result = sub(/matrix/, "& &", test); print test; print result}'
matrix matrix
1

$ cat /tmp/test
max
maxx

$ awk '{result = sub(/x+/, "& test"); print; print result}' /tmp/test
max test
1
maxx test
1
```

#### gensub(regexp, replacement, how [, target])
Search the *target* string target for matches of the regular expression *regexp*. If *how* is a string beginning with 'g' or 'G' (short for "global"), then replace all matches of *regexp* with *replacement*. Otherwise, treat *how* as a number indicating which match of *regexp* to replace. Treat numeric values less than one as if they were one. If no *target* is supplied, use *$0*. Return the modified string as the result of the function. The original target string is *not* changed.
*gensub()* is a general substitution function. Its purpose is to provide more features than the standard *sub()* and *gsub()* functions.
*gensub()* provides an additional feature that is not available in *sub()* or *gsub()*: the ability to specify components of a regexp in the replacement text. This is done by using parentheses in the regexp to mark the components and then specifying *'\N'* in the replacement text, where *N* is a digit from 1 to 9.  

```bash
$ awk 'BEGIN{test="max zou max"; dest = gensub(/m.x/, "matrix", "1", test); print dest}'
matrix zou max
```

#### gsub(regexp, replacement [, target])
Search *target* for all of the longest, leftmost, nonoverlapping matching substrings it can find and replace them with replacement. The 'g' in gsub() stands for "global", which means replace everywhere. 
The *gsub()* function returns the number of substitutions made. If the variable to search and alter ( *target* ) is omitted, then the entire input record ( *$0* ) is used. 
```bash
$ awk 'BEGIN{test="matrix matrix"; count = gsub(/matrix/, "zou", test); print test; print count}'
zou zou
2
```

#### index(in, find)
Search the string *in* for the first occurrence of the string *find*, and return the position in characters where that occurrence begins in the string *in*. 

```bash
$ awk 'BEGIN{print index("matrix matrix", " matrix")}'                        
7

$ awk 'BEGIN{print index("matrix matrix", "  matrix")}'
0

$ awk 'BEGIN{print index("matrix matrix", "matrix")}'                         
1
```

#### length([string])
Return the number of characters in *string*. If no argument is supplied, *length()* returns the length of *$0*. When given an array argument, the *length()* function returns the number of elements in the array.

```
$ awk 'BEGIN{test = "matrix"; print length(test)}'
6

$ awk 'BEGIN{test[1] = "matrix"; test[2] = "zou"; print length(test)}'
2
```

#### match(string, regexp [, array])
Search *string* for the longest, leftmost substring matched by the regular expression *regexp* and return the character position (index) at which that substring begins (one, if it starts at the beginning of *string*). If no match is found, return zero.
The *regexp* argument may be either a regexp constant (/…/) or a string constant ("…").
The order of the first two arguments is the opposite of most other string functions that work with regular expressions, such as *sub()* and *gsub()*. It might help to remember that for *match()*, the order is the same as for the ‘~’ operator: ‘string ~ regexp’.
The match() function sets the predefined variable RSTART to the index. It also sets the predefined variable RLENGTH to the length in characters of the matched substring. If no match is found, RSTART is set to zero, and RLENGTH to -1. 

```bash
$ awk 'BEGIN{test = "matrix"; print match(test, /t.*x/); print RLENGTH, RSTART}'
3
4 3
```

If *array* is present, it is cleared, and then the zeroth element of *array* is set to the entire portion of *string* matched by *regexp*. If *regexp* contains parentheses, the integer-indexed elements of *array* are set to contain the portion of *string* matching the corresponding parenthesized subexpression.   

```bash
$ awk 'BEGIN{test = "matrix zou"; match(test, /(m.*x) (z.*u)/, array); count=length(array); for(i = 0; i<=2; i++)print array[i]}'
matrix zou
matrix
zou
```

In addition, multidimensional subscripts are available providing the start index and length of each matched subexpression: 

```bash
$ awk 'BEGIN{test = "matrix zou"; match(test, /(m.*x) (z.*u)/, array); for (x in array)print x, array[x]}'
0start 1
0length 10
1start 1
2start 8
0 matrix zou
1 matrix
2 zou
2length 3
1length 6
```

#### patsplit(string, array [, fieldpat [, seps ] ])
Search *fieldpat* in  *string* and store the pieces in *array* and the separator strings in the *seps* array. The first piece is stored in *array[1]*, the second piece in *array[2]*, and so forth. The third argument, *fieldpat*, is a regexp describing the fields in *string*. It may be either a regexp constant or a string. If *fieldpat* is omitted, the value of FPAT is used. *patsplit()* returns the number of elements created. *seps[i]* is the possibly null separator string after *array[i]*. The possibly null leading separator will be in *seps[0]*. So a non-null string with *n* fields will have *n+1* separators.

```bash
$ awk 'BEGIN{patsplit("matrix|zou", a, /[a-z]*/, seps); print "array result:"; for(i=1;i<=length(a);i++)print a[i]; print length(a); print "seperator result:"; for(j=0;j<length(seps);j++) print seps[j]; print length(seps)}'
array result:
matrix
zou
2
seperator result:

|
2

$ awk 'BEGIN{patsplit("matrix|zou", a, /[^|]*/, seps); print "array result:"; for(i=1;i<=length(a);i++)print a[i]; print length(a); print "seperator result:"; for(j=0;j<length(seps);j++) print seps[j]; print length(seps)}'
array result:
matrix
zou
2
seperator result:

|
2
```
NOTE: *seps* start from *0*, instead of *1*
*regexp [^|]* refers to everything except *|* 

#### split(string, array  [, fieldsep [, seps ] ])
Divide *string* into pieces separated by *fieldsep* and store the pieces in *array* and the separator strings in the *seps* array.  The first piece is stored in *array[1]*, the second piece in *array[2]*, and so forth. The string value of the third argument, *fieldsep*, is a regexp describing where to split *string*. If *fieldsep* is omitted, the value of FS is used. *split()* returns the number of elements created. *seps* is a gawk extension, with *seps[i]* being the separator string between *array[i]* and *array[i+1]*. If *fieldsep* is a single space, then any leading whitespace goes into *seps[0]* and any trailing whitespace goes into *seps[n]*, where *n* is the return value of *split()* (i.e., the number of elements in array).
The split() function splits strings into pieces in the same way that input lines are split into fields.

```bash
$ awk 'BEGIN{split("matrix|zou", a, /[|]/, seps); print "array result:"; for(i=1;i<=length(a);i++)print a[i]; print length(a); print "seperator result:"; for(j=0;j<length(seps);j++) print seps[j]; print length(seps)}'
array result:
matrix
zou
2
seperator result:

|
2

$ awk 'BEGIN{split("matrix|zou", a, "|", seps); print "array result:"; for(i=1;i<=length(a);i++)print a[i]; print length(a); print "seperator result:"; for(j=0;j<length(seps);j++) print seps[j]; print length(seps)}'
array result:
matrix
zou
2
seperator result:

|
2
```

#### sprintf(format, expression1, …)
Return (without printing) the string that *printf* would have printed out with the same arguments. In fact, *sprintf* acts in exactly the same way as *printf*, except that *sprintf* assigns its output to a variable, not standard output.

```bash
$ awk 'BEGIN{pival = sprintf("matrix is a good boy")}'

$ awk 'BEGIN{pival = sprintf("matrix is a good boy"); print pival}'
matrix is a good boy
```

#### strtonum(str)
Examine *str* and return its numeric value. If *str* begins with a leading ‘0’, *strtonum()* assumes that str is an octal number. If *str* begins with a leading ‘0x’ or ‘0X’, *strtonum()* assumes that str is a hexadecimal number. 

```bash
$ awk 'BEGIN{val = "0x11"; print strtonum(val)}'
17
```

#### substr(string, start [, length ])
Return a *length*-character-long substring of *string*, starting at character number *start*. The first character of a string is character number one. For example, substr("washington", 5, 3) returns "ing".
If *length* is not present, *substr()* returns the whole suffix of string that begins at character number *start*. For example, substr("washington", 5) returns "ington". The whole suffix is also returned if *length* is greater than the number of characters remaining in the *string*, counting from character *start*.
If *start* is less than one, *substr()* treats it as if it was one. If *start* is greater than the number of characters in the *string*, *substr()* returns the null string. Similarly, if *length* is present but less than or equal to zero, the null string is returned. 

#### tolower(string) / toupper(string)
Return a copy of *string*, with each uppercase/lowercase character in the *string* replaced with its corresponding lowercase/uppercase character. Nonalphabetic characters are left unchanged. For example, tolower("MiXeD cAsE 123") returns "mixed case 123", and toupper("MiXeD cAsE 123") returns "MIXED CASE 123". 

### Customised Function
```bash
function name (parameter-list) {
	statements
}
```

Examples

```bash
$ cat insert.awk
# insert.awk - insert characters on position

function insert(STRING, POS, INS) {
    before = substr(STRING, 1, POS)
    after = substr(STRING, POS+1)
    new_STRING = sprintf("%s%s%s", before, INS, after)
    return new_STRING
}

BEGIN {
    print insert("matrix", position, insertion)
}

$ awk -v position=2 -v insertion=" " -f insert.awk
ma trix
```

## Operator
**<** Less than  
**>** Greater than  
**<=** Less than or equal to >= Greater than or equal to == Equal to  
**!=** Not equal to  
**~** Matches  
**!~** Does not match  
**||** Logical OR   
**&&** Logical AND    
**!** Logical NOT   

{% include links.html %}
