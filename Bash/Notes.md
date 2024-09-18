# Bash Scripting
## Commands
-  man        #manual pages for linux
-  touch      #create a file
-  mkdir      #create a directory
-  umask      #default permissions
-  ls -lisa   #list items
-  vls -1     #one item per line
-  rm         #remove files
-  -r or -rf  #remove files
-  rmdir      #remove directory
-  pwd        #show directory
-  mv         #moving files
-  cp         #copy

### reading files
-  cat
-  more
-  head
-  less
-  tail

### Commands to Search for files
```
find -name <case sentsitive name>
find -iname <non case sentitive name>
find -inum <an inode number>
find -size <size of file>
find -size -1g <find a file less than 1 gig>
find -size +1g <find a file biggern than 1 gig>
find -gid <group id>
find -uid <user id>
find -maxdepth <define how deep to go in a directory as a number>
find -type d  #finds all of the directories f is for files, p is for pipes, 
find -name \*.txt #find all .txt files in the directory
```
```
find -atime 3  #anything accessed in the last 3 days
find -ctime 3  #anything changed in the last 3 days
find -mtime 3  #anything modified in the last 3 days
find -amin 60  #anything accessed in the last 60 minutes
find -cmin 60  #anything changed in the last 60 minutes
find -mmin 60  #anything modified in the last 60 minutes
find $HOME -mtime 0  #everything in the last 24 hours in the home directory of current user
```
### Example
```
find / -perm /4000 -uid 0 -ls 2>/dev/null
find /var/log -iname *.log 2>/dev/null -printf "%i %f\n"  
-printf "%i %f\n"  #prints in a format of inode then file and new line
2>/dev/null  #error handling
```
### Searching inside of a file
```
grep  #prints the files that match a line or input
grep -E  #extended grep
egrep    #extended grep
grep -i  #insensitive match
grep -v  #inverts your search
grep -n  #line numbers
```
### Example
```
egrep "student|root|bob" fakepasswd.txt  #uses regex to find users inside the copied etc passwd file
cat fakepasswd.txt | grep -v /bin/bash  #shows everything that does not have /bin/bash
```
### Privilege escalation
```
sudo !!  #runs last command in root permissions
```
### Exec command
```
#using exec with the find command
find [path] [arguments] -exec [command] {} \;

[command] is what you want to execute over results given by the find command.
{} is a placeholder that is used to hold results given by the find command.
\; says that for each found result, the [command] is executed. You need to escape the ; here and hence \;.
There must be a space between {} and \;
```
### Process Commands
```
ps -elf
ps aux
ps
kill
```
### Cut Command
```
#cut
cat <file> | cut -d: -f1  #-d is for delimiter. -f1 is field 1
cut -d: -f1 -s
```
### Chaining Operators
```
&     #bitwise AND
&&    #logical AND
||    #ligical OR
!     #logical NOT EQUAL TO
|     #bitwise OR
^     #bitwise XOR
~     #bitwise complement
```
### Awk Command
```
cat fakepasswd | awk -F: '{print $1}'  #-F is field seperator, : is what to seperate by, 
awk -F: '{print $NF}' fakepasswd  #NF prints only last field of every line
awk -F: '($3 == 0) {print $1}' fakepasswd
awk -F: 'begin {OFS="@"} {print $1, $3}' fakepasswd
```
### Examples
```
awk -F: 'begin {OFS=":"} {print $1, $3}' fakepasswd.txt
cat /etc/passwd | awk -F: '($3 >= 150){print $0}'
cat /etc/passwd | awk -F: '($3 >= 150){print $1, $6, $3}'
cat /etc/passwd | awk -F: '($7 == "/bin/bash"){print $1, $6, $3}'
cat /etc/passwd | awk -F: '($7 == "/usr/sbin/nologin"){print $1, $6, $3}'
```
### FG Examples
```
awk -F: '{print $1}'                            #displays 1st field delimited by a ":"
awk '{print $2}'                                #displays 2nd field, delimited automatically by space
awk '{print $0}'                                #displays all string data that matches
awk -F: '($3 == 0) {print $1}' /etc/passwd      #displays 1st field (username) IF the 3rd field (UID) is equal to "0"
awk '{print $NF}'                               #displays only the last field of every line
```
### Sort Command
```
awk -F: '{print $3}' fakepasswd.txt | sort -n
cat fakepasswd.txt | sort -k 2,4
cat fakepasswd.txt | sort -t : -k 7
```
### Sort FG Examples
```
sort                sorts content according to position on the ASCII table
sort -n             sorts content numerically
sort -u             sorts content uniqely
sort -nr            sorts content numerically reversed
sort -t +
sort -k 2,4         sorts 1st by content in the 2nd column, then by content in the 4th column
```
### Uniq FG Examples
```
uniq                sorts content uniqely
uniq -c             sorts content uniqely with a count reading
```

### More Awk Command Examples
```
awk -F: 'begin {OFS=":"} ($3 > 3) ($7 == "/bin/bash") {print $0}' $HOME/passwd > $HOME/SED/names.txt
awk -F: '($3 >= 4) ($7 == "/bin/bash") {print $1}' $HOME/bash/passwd 
awk -F: '($3 > 3) && ($7 == "/bin/bash") {print $1}' $HOME/passwd > $HOME/SED/names.txt
```
### Dmesg example
```
dmesg -k | grep 'CPU\|BIOS' | 
usable or reserved
dmesg | grep -i -E 'CPU|BIOS' | grep -iv -E 'usable|reserved' | awk -F] '{print $NF}'
```
### Alias Examples
```
alias vim = 'nano'
| awk -F: '{print $2}'
```
### Alias FG Examples
```
alias rm='rm -i'                    #creates alias to confirm removal
alias vim='nano'                    #creates an alias for `nano'
alias gedit='nano'                  #"
alias vi='nano'                     #"
alias x='cat etc/passwd'            #creates an alias for Command: cat/etc/passwd
alias y=$(cat /etc/shadow)          #" cat /etc/shadow
alias ls='ls -al'                   #creates an alias causing 'ls -al' to be run when 'ls' is used
\ls                                 #negates the alias function, so we can run 'ls' without '-al'
alias -p                            #view all aliases set (local and global)
unalias ls                          #unaliases ls so it no longer resolves to 'ls -al'
```
### Sed Command Examples
- sed is a text editor command
```
sed -e 's/chicken/hamburger/g' -e 's/pepperoni/sausage/' pizza.txt
```
- /g is the global
- s/ is the start of the expression
- looking for chicken as the start and replace with hamburger
- /d is the delete and runs globally.
- use the global expression if you want it to change every instance of the word instead of just the first word.

### Sed FG Examples
```
sed '/.dll/{x;p;p;p;x}' -i document.txt               #directly alters document.txt by adding 3 empty lines before the designated regex (.dll)
sed '/stuff/G;/stuff/G' -i document.txt               #directly alters document.txt by adding 2 empty lines after the designated regex (stuff)
sed -i -e 's/ANCHOVIES/SAUSAGE/g' pizaster.htm        #replaces every instance of "ANCHOVIES" with "SAUSAGE" on pizaster.htm
sed -i -e 's/ANCHOVIES//g' pizaster.htm               #removes every instance of "ANCHOVIES" on pizaster.htm
sed -i '/^#/d' /etc/hosts.allow                       #removes all lines starting with "#" from file /etc/hosts.allow
```
### Command Substituion
```
#!/bin/bash
A=$(find /usr/bin -name passwd)
echo $A
echo md5sum $A
echo file $A
```
### Substituiong FG Examples
```
A=$(Command)            A=$(cat /etc/passwd)
`Command`               `cat /etc/passwd
A=value                 A=120
B=value                 B=20
expr $A - $B            =100  
C=$(expr $A - $B)
echo $C                 =100
D=.mp3                  for x in $(cat songs); do sed -i "s/$/$D/"; done
                        =appends .mp3 to the end of every song in file: songs
```

### Helpful examples
```
begin {OFS=" "}
dmesg | grep -i -E 'CPU|BIOS' | grep -iv -E 'usable|reserved' | awk -F] '{print $2}' 
dmesg | grep -i -E 'CPU|BIOS' | grep -iv -E 'usable|reserved' | awk -F] 'begin {OFS=" "} {print $NF}' 
dmesg | grep -i -E 'CPU|BIOS' | grep -iv -E 'usable|reserved' | awk -F] '{print $2}' | awk -F: '{print $2}'
dmesg | grep -i -E 'CPU|BIOS' | grep -iv -E 'usable|reserved' | cut -c 16-
```
### Tar command
```
tar -czf  #compresses file
#-c creates an archive
#-z enables compression with gzip
#-f specifies the filename
tar -cvf  #creating an uncompressed tar archive
```
### Helpful examples
```
#!/bin/bash

A=120
B=20
expr $A - $B
C=$(expr $A - $B)
echo $C
```
```
#!/bin/bash

contents=$(cat simple.txt)
if [[ $contents == "tacos" ]]; then
  echo "are good on tuesdays"
elif [[ $contents == "costco is amazing" ]]; then
  echo "and will save you money"
elif [[ $contents == "chicken bake" ]]; then
  echo "tasty but will make you fail ht/wt"
else
  echo "no tax at commissary"
fi
```





























