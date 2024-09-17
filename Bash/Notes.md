# Bash Scripting
################################RESOURCES###################################
#student guide
https://cted.cybbh.io/tech-college/pns/public/pns/latest/guides/bash_sg.html
#Website
ss64
https://linuxhandbook.com/find-exec-command/
https://phoenixnap.com/kb/bash-printf
https://www.baeldung.com/linux/bash-single-vs-double-brackets#:~:text=The%20single%20bracket%20is%20a,brackets%20is%20generally%20more%20convenient.
https://regex101.com/library

'''
'''
################################COMMANDS###################################
#manual for linux
man

#create a file
touch

#create a directory
mkdir

#default permissions
umask

#list items
ls -lisa
#one item per line
ls -1


#remove files
rm 
-r or -rf
#remove directory
rmdir

#show directory
pwd

#moving files
#copy
cp
mv

#reading files
cat
more
head
less
tail
'''
'''
################################SEARCHING###################################

#Finding things
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


find -atime 3  #anything accessed in the last 3 days
find -ctime 3  #anything changed in the last 3 days
find -mtime 3  #anything modified in the last 3 days
find -amin 60  #anything accessed in the last 60 minutes
find -cmin 60  #anything changed in the last 60 minutes
find -mmin 60  #anything modified in the last 60 minutes
find $HOME -mtime 0  #everything in the last 24 hours in the home directory of current user


find / -perm /4000 -uid 0 -ls 2>/dev/null
find /var/log -iname *.log 2>/dev/null -printf "%i %f\n"  

-printf "%i %f\n"  #prints in a format of inode then file and new line
2>/dev/null  #error handling


#finding in files
grep  #prints the files that match a line or input
grep -E  #extended grep
egrep    #extended grep
grep -i  #insensitive match
grep -v  #inverts your search
grep -n  #line numbers

egrep "student|root|bob" fakepasswd.txt  #uses regex to find users inside the copied etc passwd file

cat fakepasswd.txt | grep -v /bin/bash  #shows everything that does not have /bin/bash




sudo !!  #runs last command in root permissions


#using exec with the find command
find [path] [arguments] -exec [command] {} \;

[command] is what you want to execute over results given by the find command.
{} is a placeholder that is used to hold results given by the find command.
\; says that for each found result, the [command] is executed. You need to escape the ; here and hence \;.
There must be a space between {} and \;
'''
'''
################################PROCESSES###################################
#processes
ps -elf
ps aux
ps
kill
'''
'''
################################CUT###################################

#cut
cat <file> | cut -d: -f1
#-d is for delimiter. -f1 is field 1
cut -d: -f1 -s
'''
'''
################################CHAINING OPERATORS###################################

&     #bitwise AND
&&    #logical AND
||    #ligical OR
!     #logical NOT EQUAL TO
|     #bitwise OR
^     #bitwise XOR
~     #bitwise complement
'''
'''
################################CHALLENGES1###################################
#1
Brace expansion is a mechanism by which arbitrary strings may be generated, for commands that will take multiple arguements. For the below examples, the first example is equivalent to the second command.

$ mkdir /var/log/{auth,syslog,dmesg}_log

Results in

$ mkdir /var/log/auth_log /var/log/syslog_log /var/log/dmesg_log

Activity: Using Brace-Expansion, create the following directories within the $HOME directory:

1123
1134
1145
1156
#answer
mkdir 11{23,34,45,56}
#2
As we learned, the following example would create five files with one command.

touch file1.txt file2.txt file3.txt passwd.txt shadow.txt

But, with Brace Expansion it can be shortened to the following.

touch file{1..3}.txt passwd.txt shadow.txt

Activity:

Use Brace-Expansion to create the following files within the $HOME/1123 directory. You may need to create the $HOME/1123 directory. Make the following files, but utilze Brace Expansion to make all nine files with one touch command.

Files to create:
################################CHAINING OPERATORS###################################

1.txt
2.txt
3.txt
4.txt
5.txt
6~.txt
7~.txt
8~.txt
9~.txt
#answer
touch $HOME/1123/{1..5}.txt $HOME/1123/{6..9}~.txt
#3
Activity:

Using the find command, list all files in $HOME/1123 that end in .txt.

Be aware that if you use Pattern Matching to locate the files you may have unintended results based on if you use quotes around the pattern or not. If you do not quote the pattern, the Bash shell interprets the pattern. If you quote the pattern, it is passed to the command for it to interpret. You can have a properly functioning command, yet unintended output, based on which of these two gets to interpret the pattern.
#answer
find $HOME/1123 -name \*.txt
#4
Challenge Activity:

List all files in $HOME/1123 that end in .txt. Omit the files containing a tilde (~) character.

While this activity can be accomplished with only find, it can also be combined with grep as well.
#answer
find -name \*.txt | grep -v ~.txt
################################END OF CHALLENGES###################################
'''
'''
################################CHALLENGES2###################################
#1
Copy all files in the $HOME/1123 directory, that end in ".txt", and omit files containing a tilde "~" character, to directory $HOME/CUT.

Use only the find and cp commands. You will need to utilize the -exec option on find to accomplish this activity.
-c
The find command uses BOOLEAN "!" to designate that it does not want to find any files or directories that follows.
#answer
find $HOME/1123 -name *.txt ! -name *~* -exec cp {} $HOME/CUT \;
#2
Using ONLY the find command, find all empty files/directories in directory /var and print out ONLY the filename (not absolute path), and the inode number, separated by newlines.

Tip: When using the man pages, it is better to focus your search then tofind visually scan 1000+ lines of text. Combining the output with the grep command, possibly with its -A, -B, or -C options, can help drive context driven searches of those manual pages.

Example Output

123 file1
456 file2
789 file3
#answer
find /var -empty -printf "%i %f\n"
#3
Using ONLY the find command, find all files on the system with inode 4026532575 and print only the filename to the screen, not the absolute path to the file, separating each filename with a newline. Ensure unneeded output is not visible.

Tip: The above inode is specific to this CTFd question and might not be in use on your Linux Opstation. Instead, you can test your command on your Linux OpStation against inode 999.
#answer
find / -type f -inum 4026532575 -printf "%f\n"
#4
Using only the ls -l and cut Commands, write a BASH script that shows all filenames with extensions ie: 1.txt, etc., but no directories, in $HOME/CUT.
Write those to a text file called names in $HOME/CUT directory.
Omit the names filename from your output.
NOTE: The output should only be the file names with no additional information. Additionally, your code will be executed twice. This is to ensure you have taken into account how file redirection and command execution works.
#answer
ls -l $HOME/CUT | cut -d: -s -f2 | cut -d' ' -f2 -s | cut -d. -f1-2 -s > $HOME/CUT/names
'''
'''
################################DAY 2###################################
#awk
cat fakepasswd | awk -F: '{print $1}'
#-F is field seperator, : is what to seperate by, 
awk -F: '{print $NF}' fakepasswd
#NF prints only last field of every line
awk -F: '($3 == 0) {print $1}' fakepasswd
#
awk -F: 'begin {OFS="@"} {print $1, $3}' fakepasswd
################################CHALLENGES2###################################

awk -F: 'begin {OFS=":"} {print $1, $3}' fakepasswd.txt
cat /etc/passwd | awk -F: '($3 >= 150){print $0}'
cat /etc/passwd | awk -F: '($3 >= 150){print $1, $6, $3}'
cat /etc/passwd | awk -F: '($7 == "/bin/bash"){print $1, $6, $3}'
cat /etc/passwd | awk -F: '($7 == "/usr/sbin/nologin"){print $1, $6, $3}'


'''
'''
################################GUIDE EXAMPLES###################################

awk -F: '{print $1}'         displays 1st field delimited by a ":"
awk '{print $2}'             displays 2nd field, delimited automatically by space
awk '{print $0}'             displays all string data that matches

awk -F: '($3 == 0) {print $1}' /etc/passwd
                            displays 1st field (username) IF the 3rd field (UID) is equal to "0"
awk '{print $NF}'           displays only the last field of every line

'''
'''
################################SORT###################################
awk -F: '{print $3}' fakepasswd.txt | sort -n
cat fakepasswd.txt | sort -k 2,4
cat fakepasswd.txt | sort -t : -k 7



'''
'''
################################GUIDE EXAMPLES###################################


sort                sorts content according to position on the ASCII table
sort -n             sorts content numerically
sort -u             sorts content uniqely
sort -nr            sorts content numerically reversed
sort -t +
sort -k 2,4         sorts 1st by content in the 2nd column, then by content in the 4th column

'''
'''
################################UNIQ###################################
################################GUIDE EXAMPLES###################################

uniq                sorts content uniqely
uniq -c             sorts content uniqely with a count reading
'''

awk -F: 'begin {OFS=":"} ($3 > 3) ($7 == "/bin/bash") {print $0}' $HOME/passwd > $HOME/SED/names.txt
awk -F: '($3 >= 4) ($7 == "/bin/bash") {print $1}' $HOME/bash/passwd 


awk -F: '($3 > 3) && ($7 == "/bin/bash") {print $1}' $HOME/passwd > $HOME/SED/names.txt

dmesg -k | grep 'CPU\|BIOS' | 
usable or reserved

dmesg | grep -i -E 'CPU|BIOS' | grep -iv -E 'usable|reserved' | awk -F] '{print $NF}'

################################ALIAS###################################
alias vim = 'nano'
| awk -F: '{print $2}'
################################GUIDE EXAMPLES###################################

alias rm='rm -i'                    creates alias to confirm removal
alias vim='nano'                    creates an alias for `nano'
alias gedit='nano'                  "
alias vi='nano'                     "
alias x='cat etc/passwd'            creates an alias for Command: cat/etc/passwd
alias y=$(cat /etc/shadow)          " cat /etc/shadow
alias ls='ls -al'                   creates an alias causing 'ls -al' to be run when 'ls' is used
\ls                                 negates the alias function, so we can run 'ls' without '-al'
alias -p                            view all aliases set (local and global)
unalias ls                          unaliases ls so it no longer resolves to 'ls -al'

################################SED###################################
#sed is a text editor command
sed -e 's/chicken/hamburger/g' -e 's/pepperoni/sausage/' pizza.txt
/g is the global
s/ is the start of the expression
looking for chicken as the start and replace with hamburger
/d is the delete and runs globally.

#use the global expression if you want it to change every instance of the word instead of just the first word.





################################GUIDE EXAMPLES###################################

sed '/.dll/{x;p;p;p;x}' -i document.txt
    directly alters document.txt by adding 3 empty lines before the designated regex (.dll)

sed '/stuff/G;/stuff/G' -i document.txt
    directly alters document.txt by adding 2 empty lines after the designated regex (stuff)

sed -i -e 's/ANCHOVIES/SAUSAGE/g' pizaster.htm
            replaces every instance of "ANCHOVIES" with "SAUSAGE" on pizaster.htm

sed -i -e 's/ANCHOVIES//g' pizaster.htm
            removes every instance of "ANCHOVIES" on pizaster.htm

sed -i '/^#/d' /etc/hosts.allow
            removes all lines starting with "#" from file /etc/hosts.allow



################################COMMAND SUBSTITUTION###################################
#!/bin/bash
A=$(find /usr/bin -name passwd)
echo $A
echo md5sum $A
echo file $A








################################GUIDE EXAMPLES###################################
A=$(Command)            A=$(cat /etc/passwd)
`Command`               `cat /etc/passwd


A=value                 A=120
B=value                 B=20
expr $A - $B            =100  
C=$(expr $A - $B)
echo $C                 =100
D=.mp3                  for x in $(cat songs); do sed -i "s/$/$D/"; done
                        =appends .mp3 to the end of every song in file: songs





begin {OFS=" "}


dmesg | grep -i -E 'CPU|BIOS' | grep -iv -E 'usable|reserved' | awk -F] '{print $2}' 
dmesg | grep -i -E 'CPU|BIOS' | grep -iv -E 'usable|reserved' | awk -F] 'begin {OFS=" "} {print $NF}' 
dmesg | grep -i -E 'CPU|BIOS' | grep -iv -E 'usable|reserved' | awk -F] '{print $2}' | awk -F: '{print $2}'
dmesg | grep -i -E 'CPU|BIOS' | grep -iv -E 'usable|reserved' | cut -c 16-


################################CHALLENGES###################################
#1
Write a basic bash script that greps ONLY the IP addresses in the text file provided (named StoryHiddenIPs in the current directory); sort them uniquely by number of times they appear.

It is not important to have a regular expression that only catches fully valid IP addresses. It is more important that you become familiar with creating and using regular expressions. 
Below, there are some useful websites that you can use to visually see what your regular expression pattern is matching on.
#answer
grep -o "[0-9]\{1,3\}\.[0-9]\{1,3\}\.[0-9]\{1,3\}\.[0-9]\{1,3\}" StoryHiddenIPs | sort | uniq -c | sort -nr
#2
Using ONLY the awk command, write a BASH one-liner script that extracts ONLY the names of all the system and user accounts that are not UIDs 0-3.
Only display those that use /bin/bash as their default shell.
The input file is named $HOME/passwd and is located in the current directory.
Output the results to a file called $HOME/SED/names.txt
#answer
awk -F: '($3 > 3) && ($7 == "/bin/bash") {print $1}' $HOME/passwd > $HOME/SED/names.txt
#3
Find all dmesg kernel messages that contain CPU or BIOS (uppercase) in the string, but not usable or reserved (case-insensitive)
Print only the msg itself, omitting the bracketed numerical expressions ie: [1.132775]
#answer
dmesg | grep -E 'CPU|BIOS' | grep -i -v -E 'usable|reserved' | cut -c 15-
#4
Write a Bash script using "Command Substitution" to replace all passwords, using openssl, from the file $HOME/PASS/shadow.txt with the MD5 encrypted password: Password1234, with salt: bad4u
Output of this command should go to the screen/standard output.
#answer
#!/bin/bash
A=$(openssl passwd -1 -salt bad4u Password1234)
awk -F: -v "awk_var=$A" 'BEGIN {OFS=":"} {$2=awk_var} {print}' $HOME/PASS/shadow.txt
#5
Using ONLY sed, write all lines from $HOME/passwd into $HOME/PASS/passwd.txt that do not end with either /bin/sh or /bin/false.
#answer
sed -e '/\/bin\/sh/d' -e '/\/bin\/false/d' $HOME/passwd > $HOME/PASS/passwd.txt


################################TAR###################################
#compresses file
tar -czf
#-c creates an archive
#-z ???
#-f specifies the filename

#creating an uncompressed tar archive
tar -cvf



################################DAY 3###################################
#!/bin/bash

A=120
B=20
expr $A - $B
C=$(expr $A - $B)
echo $C

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






























