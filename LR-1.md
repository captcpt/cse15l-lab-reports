# Lab Report 1

## Commands with no arguments
#### cd
```bash
[user@sahara ~]$ pwd
/home
[user@sahara ~]$ cd
```
Since I was not in any specific directory, it returned "/home" because I was in the home directory with no argument to change directory to (and because was in base directory, did not move me anywhere although if I was in another directory like lecture1, it would have moved me back to the home directory). 

#### ls
```bash
[user@sahara ~]$ pwd
/home
[user@sahara ~]$ ls
lecture1
```
The output of lecture1 directory occurred because from the home directory, it was the only object present to list.

#### cat
```bash
[user@sahara ~]$ pwd
/home
[user@sahara ~]$ cat

```

Since no argument was given, *cat* printed a blank output (since the contents of nothing is nothing).

## Commands with directory arguments
#### cd
```bash
[user@sahara ~]$ pwd
/home
[user@sahara ~]$ cd lecture1
[user@sahara ~]$ pwd
/home/lecture1
[user@sahara ~/lecture1]$
```

By using the argument of "lecture1," the command effectively changed the working directory to /home/lecture1 as seen using pwd.

#### ls
```bash
[user@sahara ~]$ pwd
/home
[user@sahara ~]$ ls lecture1
Hello.class Hello.java messages README
[user@sahara ~]$
```

Using the argument of the lecture1 directory, the command listed all objects found within "lecture1."

#### cat
```bash
[user@sahara ~]$ pwd
/home
[user@sahara ~]$ cat lecture1
cat: lecture1: Is a directory
```

By using *cat* on a directory (lecture1), it would first print the name of the argument provided (lecture1: ) and then the contents of lecture1 would be labeled "Is a directory," since it does not have any other details.

## Commands with file arguments
#### cd
```bash
[user@sahara ~]$ pwd
/home/lecture1
[user@sahara ~]$ cd README
bash: cd: README: Not a directory
[user@sahara ~]$
```

By attempting to use *cd* on a file (README), it returned an error because the directory cannot be changed to a static object (must be a directory).

#### ls
```bash
[user@sahara ~]$ pwd
/home/lecture1
[user@sahara ~]$ ls README
README
[user@sahara ~]$
```
Using *ls* on README listed back the README file since it is the only object present (in itself).

#### cat
```bash
[user@sahara ~]$ pwd
/home/lecture1
[user@sahara ~]$ cat README
To use this program:

javac Hello.java
java Hello messages/en-us.txt
[user@sahara ~]$
```

Using *cat* on the README file returns the contents (text) which provides information about using the program (as seen in the text printed).



