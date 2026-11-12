This module is about the basics of Linux file paths.

## The Root

Alright, so the filesystem starts at /. Under that, there are a whole mess of other directories, configuration files, programs, and, most importantly, flags. In this level, we've added a program right in /, called pwn, that will give you the flag. All you need to do for this level is to invoke this program!

You can invoke a program by providing its path on the command line. In this case, you'll be giving the exact path, starting from /, so the path would be /pwn. This style of path, one that starts with the root directory, is referred to as an "absolute path".

#### Solution

/pwn

#### What did i learn

`The filesystem starts at "/".`

## Programs and absolute paths

Let's explore a slightly more complicated path! Except for in the previous level, challenges in pwn.college are in the challenge directory and the challenge directory is, in turn, right in the root directory (/). The path to the challenge directory is, thus,
/challenge. The name of the challenge program in this level is run, and it lives in the /challenge directory. Thus, the path to the run challenge program is /challenge/run.

#### Solution

/challenge/run

#### What did i learn

`Invoking an absolute path.`

## Position thy self

The Linux filesystem has tons of directories with tons of files. You can navigate around directories by using the cd (change directory) command and passing a path to it as an argument, as so:

hacker@dojo:~$ cd /some/new/directory
hacker@dojo:/some/new/directory$

This affects the "current working directory" of your process (in this case, the bash shell). Each process has a directory in which it's currently hanging out. The reasons for this will become clear later in the module.

As an aside, now you can see what the ~ was in the prompt! It shows the current path that your shell is located at.

#### Solution

/challenge/run
You are not currently in the /etc directory.
Please use the `cd` utility to change directory appropriately.
cd /etc
/challenge/run

#### What did i learn

`The built in command 'cd' which stands for (change directory) for navigation through directories`

## Position elsewhere

Same thing, but 5 times!

#### Solution

Changing directories 5 times invoking the /challenge/run

#### What did i learn

`Practicing 'cd'`

## Implicit relative paths, from /

Now you're familiar with the concept of referring to absolute paths and changing directories. If you put in absolute paths everywhere, then it really doesn't matter what directory you are in, as you likely found out in the previous three challenges.

However, the current working directory does matter for relative paths.

    A relative path is any path that does not start at root (i.e., it does not start with /).
    A relative path is interpreted relative to your current working directory (cwd).
    Your cwd is the directory that your prompt is currently located at.

This means how you specify a particular file, depends on where the terminal prompt is located.

Imagine we want to access some file located at /tmp/a/b/my_file.

    If my cwd is /, then a relative path to the file is tmp/a/b/my_file.
    If my cwd is /tmp, then a relative path to the file is a/b/my_file.
    If my cwd is /tmp/a/b/c, then a relative path to the file is ../my_file. The .. refers to the parent directory.

Changing directories and invoking a program are two separate actions. For example:

hacker@dojo:~$ cd /
hacker@dojo:/$ bin/echo Hello
Hello

The first command changes the current working directory to /. The second command invokes /bin/echo using the relative path bin/echo.

#### Solution

cd /
challenge/run

#### What did i learn

`You can invoke programs without using the full absolute path if you are in the cwd (current working directory) where the program is located. So if the program is in '/' and the cwd is '/' you can continue to the path of the program without using the '/'.`

## Explicit relative paths, from /

Previously, your relative path was "naked": it directly specified the directory to descend into from the current directory. In this level, we're going to explore more explicit relative paths.

In most operating systems, including Linux, every directory has two implicit entries that you can reference in paths: . and ... The first, ., refers right to the same directory, so the following absolute paths are all identical to each other:

    /challenge
    /challenge/.
    /challenge/./././././././././
    /./././challenge/././

The following relative paths are also all identical to each other:

    challenge
    ./challenge
    ./././challenge
    challenge/.

Of course, if your current working directory is /, the above relative paths are equivalent to the above absolute paths.

#### Solution

cd /
./challenge/run

#### What did i learn

` '.' means "the current directory." So './challenge/run' means "start in the folder I am in now, then go into challenge, then run." Because it starts from the current directory, I must be in '/' first (cd /). Adding './' at the start does not change the path, it only makes it clear that the path is relative.`

## Implicit relative paths

In this level, we'll practice referring to paths using . a bit more. This challenge will need you to run it from the /challenge directory. Here, things get slightly tricky.

Linux explicitly avoids automatically looking in the current directory when you provide a "naked" path. Consider the following:

hacker@dojo:~$ cd /challenge
hacker@dojo:/challenge$ run

This will not invoke /challenge/run. This is actually a safety measure: if Linux searched the current directory for programs every time you entered a naked path, you could accidentally execute programs in your current directory that happened to have the same names as core system utilities! As a result, the above commands will yield the following error:

bash: run: command not found

We'll explore the mechanisms behind this concept later, but in this challenge, we'll learn how to explicitly use relative paths to launch run in this scenario. The way to do this is to tell Linux that you explicitly want to execute a program in the current directory, using . like in the previous levels.

#### Solution

cd /challenge
./run

#### What did i learn

`Typing just run does not work, even if I am in the folder where run is. Linux only looks for programs in certain system folders, not in my current folder. This is a safety rule, so I don't run a program by accident. To run a program in my current folder, I must write ./run. The `.` means "the current directory," so ./run clearly says "run the file named run that is in this folder."`

## Home sweet home

Every user has a home directory, typically under /home in the filesystem. In the dojo, you are the hacker user, and your home directory is /home/hacker. The home directory is typically where users store most of their personal files. As you make your way through pwn.college, this is where you'll store most of your solutions.

Typically, your shell session will start with your home directory as your current working directory. Consider the initial prompt:

hacker@dojo:~$

The ~ in this prompt is the current working directory, with ~ being shorthand for /home/hacker. Bash provides and uses this shorthand because, again, most of your time will be spent in your home directory. Thus, whenever bash sees ~ provided as the start of an argument in a way consistent with a path, it will expand it to your home directory. Consider:

hacker@dojo:~$ echo LOOK: ~
LOOK: /home/hacker
hacker@dojo:~$ cd /
hacker@dojo:/$ cd ~
hacker@dojo:~$ cd ~/asdf
hacker@dojo:~/asdf$ cd ~/asdf
hacker@dojo:~/asdf$ cd ~
hacker@dojo:~$ cd /home/hacker/asdf
hacker@dojo:~/asdf$

Note that the expansion of ~ is an absolute path, and only the leading ~ is expanded. This means, for example, that ~/~ will be expanded to /home/hacker/~ rather than /home/hacker/home/hacker.

Fun fact: cd will use your home directory as the default destination:

hacker@dojo:~$ cd /tmp
hacker@dojo:/tmp$ cd
hacker@dojo:~$

Now it's your turn to play! In this challenge, /challenge/run will write a copy of the flag to any file you specify as an argument on the commandline, with these constraints:

    Your argument must be an absolute path.
    The path must be inside your home directory.
    Before expansion, your argument must be three characters or less.

Again, you must specify your path as an argument to /challenge/run as so:

hacker@dojo:~$ /challenge/run YOUR_PATH_HERE

#### Solution

/challenge/run ~/a

#### What did i learn

` '~' is a shortcut for my home directory, /home/hacker. Bash replaces it with the full absolute path before the program runs. This lets me write a very short path like ~/a (3 characters) that becomes /home/hacker/a. Only the ~ at the start is replaced. Also, cd with no argument takes me to my home directory.`