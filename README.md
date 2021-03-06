# Learning The Command Line

The _shell_ takes keyboard commands, passing them to the operating system to executed. A Terminal Emulator is a GUI to interact with the shell.

Command history can be accessed by pressing the up arrow key, left and right will move the cursor through the currently typed command.

Copy text by highliting it, paste with middle mouse button (or cmd + v)


## Let's Get Moving

Unix operating systems organize their files in a _hierachical directory structure_, fancy words for a tree like pattern of directories (folders) containing files and/or more directories. The first is called the _root directory_. Unlike Windows, Unix systems have a single file system tree, storage devices are mounted at points on the tree by sysadmins (people responsible for maintenance).

Display where you are sitting on the tree by typing `pwd`, this gives you the path from root directory to where you currently are, known as the _working directory_. When you first open the terminal you will be placed in your _home directory_. Regular users can only write files in their home directory.

List the files and directories available to you with `ls`.

Move around the tree using `cd $PATHNAME`. `$PATHNAME` is the route along which you must travel to get where you want to go, there are two types Absolute and Relative.

Absolute pathnames begin in the root directory, if you know the path from the root directory you could use this from anywhere on the tree and reach the same destination. Absolute pathnames start with a `/`, typing `cd /` would take you to the root directory, `cd /Users/$USERNAME` (where `$USERNAME` is your account username on the machine) would take you to your home directory. These are all Absolute pathnames.

Relative pathnames begin in the working directory. If this is not the root directory and you want to back out of a directory (towards the root) you can use dot `.` symbols to do so. A single dot `.` represents the working directory, two dots `..` represents the directory abbove you. Of course you can use `pwd` to find the absolute pathname and use that to move up a directory, but that's a hassle. So, an Relative pathname would start in the working directory and you can type it out like this `cd ./Documents`. You don't actually need the `./`, it is generally implied.

Helpful shortcuts
|Command | Result |
|--------|--------|
| `cd ~` | Move to home directory|
| `cd -` | Move to previous working directory|

### Things to Remember

Files beginning with a `.` are _hidden_ and will not be revealed with `ls` unless flagged with `-a`.

Filenames are _not_ case sensitive, after creating `file1.txt` you can open it by referencing `File1` (the opposite is true for Linux).

Try not to use spaces in filenames, it makes life harder for yourself. Use hyphens or underscores instead.

## Moving + Looking = Exploring

Now you can move around the filesystem, but that's not much use if you can't enjoy the scenery. We already know `ls` will display the working directories contents. `file $FILENAME` will tell you the the file type. `less` will provide you with the file contents (if you just tried it press `q` to exit).

Pathnames aren't just for chaning directory, `ls $PATHNAME` will give you the contents of the directory at the end of the path. `ls` can even take multiple pathnames at once `ls ~ .` will list the contents of your home directory and current directory (it will display them seperately rather than as one mass of files).

Options (short for optional) can follow from commands, modifying their behaviour. Flags are a specific type of option, boolean and defaulting to false. One such flag was mentioned earlier, `-a` will modify the behaviour of the `ls` command, such that `ls -a` will display hidden files (files starting with a `.`). `ls -l` is another flag, this one will display additional information about each file such as read/write access, creator, etc.

Here are some of the options for the `ls` command

| Option | description |
|--------|-------------|
| `-a`   | list all files |
| `-l`   | display long format |
| `-d`   | use in conjunction with `-l` to see directory details rather than contents |
| `-F`   | append indicator character to end of each name |
| `-h`   | use in conjunction with `-l` to see file sizes in more appropriate units |
| `-r`   | display results in reverse order |
| `-S`   | sort results by file size |
| `-t`   | sort results by modification time |

### What do your `-lf` Eyes See?

That's not even all the options, but let's leave it there for now. I'm sure you tried a few, the `-l` option will have spit out all sorts of stuff and it isn't obvious what it all means so let's try to explain each field. 

```bash
$ ls -l
-rw-r--r--  1 uniqueuser1  staff  4119 13 Mar 16:22 README.md
```

`-rw-r--r--` is a summary of the access rights to the file. The first character denotes file type (`-` in this case for a regular file). The next three characters indicate read, write, and execute permissions for the file owner respectively. The next three denote the same for members of the files group, the final three for everyone else.

`1` Files number of hard links, discussed in a bit

`uniqueuser1` File owner user name

`staff` Name of group owning the file

`4119` File size in bytes

`13 Mar 17:14` Time of last modification

`README.md` Name of file (hey, it's this file!)

### Don't assume my file type

All you need to do is ask! `file $FILENAME` will give you the file type. This may not seem very useful but I'm sure we can find some applications for it later, there's bound to be all sorts of flags/options for it right?

### Final `less`ons in Exploration

You'll be coming across a lot of text files while traversing the directory tree. Fortunately there is a handy command for peeking into text files while in the shell `less`. 

`less $FILENAME` will open the text file, allowing you to scroll through it. The keyboard can be used to navigate through a variety of commands.

| Command | Action |
|---------|--------|
| B       | Scroll up one page |
| Spacebar| Scroll down one page |
| Up Arrow| Scroll up one line |
| Down Arrow| Scroll down one line |
| G       | Sroll to file end |
| 1G or g | Move to file start |
| /characters| Search for occurrences |
| n       | Next occurrence of last search |
| h       | Help screen |
| q       | Quit `less` |

There are a load more commands along with lots of options to use with the `less` program, but I'm bored of writing about it so let's move on.

If you've been doing some exploring of your own you may have found something called a "Symbolic Link", you can discern this from the access rights column (`ls -l`) because the first charachter will be an `l`, for example `lrwxrwxrwx`. These are used to reference a file in another location and can be very useful. Let's not dwell on it, you're about to recieve your next set of powers.

## God created Man, Man created File Systems

Moving and Seeing isn't enough for you eh? You want to get your hands dirty? really feel those files between your fingers? It's time to learn the powers of creation and destruction, also known as `mkdir` and `rmdir`. Try not to chop down the whole tree!

Some of the commands you are going to learn here are actually easier to execute using the GUI, but what the command line offers is flexibility. For example, moving a file from one place to another is a simple drag and drop operation when using the GUI but what about moving all files of a certain type, or just files which don't already exist in the new location? Well you could scroll through each directory selecting all files ending `.html` or you could just type `cp *.html $DESTINATION` and have it done in a flash.

What's with the asterix? you may be asking. It's what we in the biz call a _wildcard_, a special character used to select files based on patterns in their name. The asterix used in the last command denotes any characters, so `*.html` would match `hello_world.html` and `goodbye_world.html` but not `hello_again.js`. Here is a table of wildcards.

| Wildcard | Matches |
|----------|---------|
| *        | Any characters|
| ?        | Any single character|
| [_characters_]| Any character in set _characters_|
|[[:_class_:]]| Any character matching _class_ |

Character classes include 

| Class | Matches |
|-------|---------|
| [:alnum:] | Any alphanumeric character |
| [:alpha:] | Any alphabetic charcter |
| [:digit:] | Any numeral |
| [:lower:] | Any lowercase letter |
| [:upper:] | Any upper case letter |

With these in your arsenal you can construct some sophisticated searches. You know that `*` means any characters, so `g*` must mean g followed by any characters. `[gh]*` would mean any file starting with g or h. Try coming up with some combinations of your own. Wildcards can also be used in the GUI, so even if you don't like using the command line, you've learned something useful.

### Make it so

Create a directory witht he `mkdir` command. `mkdir $DIRECTORY` will create a directory on the path you specify, a simple `mkdir new-dir` will create on in your working directory. You can specify a path with your directory name at the end to create it in another location, `mkdir ../new-dir` will create it in the parent of the working directory, `mkdir ~/Documents/new-dir` will create it in your Documents (remember relative and absolute paths from earlier?)

You can also make multiple directories at the same time `mkdir dir1 dir2`. You can specify different paths for each directory, just remember to seperate them by a space or you will probably get an error.

### Stop `cp`ing Me

Copy a file from one location to another with `cp`. You can give the copy a new name in the process. `cp file1.html dir1/file1renamed.html` will copy it to dir1 in the working directory and rename it. Copy multiple files at once by naming them each separated by a space.

Here are some handy options to help you out when copying files 

| Option | Meaning |
|--------|---------|
| -a     | Copy files and directories and all attributes (ownerships and permissions) |
| -i     | Prompt user before overwriting existing files|
| -r     | Recursively copy directories and contents (this or -a is required when copying directories)|
| -u     | Only files that don't exist or are newer in destination |
| -v     | Display info as copy occurs |

### `mv`ing Swiftly On

Now that you never need to copy and paste a file again, let's learn how to never need to drag and drop files. The `mv` command works very similarly to the `cp` command, except the original file is deleted. Knowing that, there isn't much more to say about it.

### The Power to Ha`rm`

You're about to learn how to delete things. This stuff doesn't get put in the recycle bin, once it's gone it's gone so be careful. This is especially the case when it comes to wildcards. With the command `rm *.html` you can delete all the html files in the working directory, all it takes is a space to delete all the files in your working directory `rm * .html`. If you're worried you may be deleting unexpected files, use the `ls` command first to see what you are removing.

Have another table of handy options

| Option | Meaning |
|--------|---------|
| -i     | Promt user before deleting file |
| -r     | Recursively delete directories (required to delete directories)|
| -f     | Do not prompt, ovverrides -i |
| -v     | Display info while deleting |

Like `cp` and `mv`, you can specify multiple files to delete at once with `rm`.

### Links (i can't think of a good title)

One last thing and then we're going to setup a little playground to try out your newfound powers. We touched on links earlier but I didn't mention that there are actually two types _Hard_ and _Symbolic_.

Hard links cannot reference a file outside its own file system nor can it reference a directory. They are indistinguishable from the file itself and directory lists will give no special indication of its presence. When a hard link is deleted the contents of the file will remain until all links to the file are deleted. Create a hard link with `ln $FILE $LINK`

Symbolic links don't have the limitations of hard links, they work by creating a file with a text pointer to the referenced file/directory. The symbolic link and the file it points to are almost indistinguishable, writing something to a symbolic link will also write it to the referenced file. Deleting a symbolic link will not delete the referenced file, deleting the file will not selete the link but the link will now point to nothing (broken link). Create a symbolic link with `ln -s $ITEM $LINK

Alright, that may have been a bit too much info so lets start experimenting. Take a look in the `playground` directory and run through the recap of what we've been over so far.

## Command It and It will Be

For a tutorial on the command line we sure have got far without actually explaining what a command is, let's clear that up. Commands can be on of four things:

- **An executable program**, these can either be *compiled binaries* or written in *scripting languages*.
- **A command built into the shell**, for example `cd`, is something called a *shell builtin*.
- **A shell function** is a miniature shell script incorporated into the *environment*.
- **An alias**, a command we have defined, uses other commands.

The `type` command mentioned earlier can be used to determine the type of a command.

```
$ type type
type is a shell builtin
$ type ls
ls is an alias for ls -G
$ type cp
cp is /bin/cp
```

The response for `type ls` may be different for you, I have an alias set up which you will elarn about in a moment.

### You need help `man`

You've been learning about the different commands and options they can be passed, but I don't expect you to come back here every time you want to remember how to use a command. Instead you can use the `man` command, try it out with whatever command you've been curious about and you'll be returned the manual on what the command can do. In bash `help`, rather than `man`, will return a short explanation in the command line.

```
$ man ls

LS(1)                     BSD General Commands Manual                    LS(1)

NAME
     ls -- list directory contents

SYNOPSIS
     ls [-ABCFGHLOPRSTUW@abcdefghiklmnopqrstuwx1] [file ...]

DESCRIPTION
     For each operand that names a file of a type other than directory, ls
     displays its name as well as any requested, associated information.  For
     each operand that names a file of type directory, ls displays the names
     of files contained within that directory, as well as any requested, asso-
     ciated information.
...
```

The returned manual uses `less` so you can use the controls discussed earlier to navigate. 

Manuals have several sections which can be accessed by being referenced in the `man` command.

| Section | Contents |
|---------|----------|
| 1       | User Commands |
| 2       | Programming interfaces for the kernel system calls |
| 3       | Programming interfaces with the C library |
| 4       | Special files e.g. nodes and drivers |
| 5       | File formats |
| 6       | Games and amusements |
| 7       | Misc. |
| 8       | Sysadmin commands |

You can also search the manual for appropriate commands with the `apropos` command. Pass a string to the command and any manual entry matching the string will be returned (in the `less` program).

```
$ apropos bash

bash(1)                  - GNU Bourne-Again SHell
bashbug(1)               - report a bug in bash
```

The number after each command is the manual section.

One more way of finding info on a command is with the `info` command. Info pages are different to man pages in that they are _hyperlinked_.

## Make a Command

Time to make a command of our own. using the `alias` command we can combine multiple commands into one and execute it by using the name we give it at creation. You can combine commands with either a semicolon (`cd /usr; ls`) or with the "and" operator (`cd /usr && ls`)

```
$ cd /usr; ls; cd -
bin        lib        libexec    local      sbin       share      standalone
~/Documents/Coding/Learning/unix-command-line
```

Be careful not to overwite an existing command, you can always delete the alias so nothing would be permanently broken. Check that the alias name you choose hasn't been taken already by using the `type` command.

```
$ type foo
foo not found
```

foo is free to use as an alias, let's use that.

```
$ alias foo='cd /usr; ls; cd -'
```

Now when you type `foo` you will run that serie of commands.

```
$ foo
bin        lib        libexec    local      sbin       share      standalone
~/Documents/Coding/Learning/unix-command-line
$ type foo
foo is aliased to `cd /usr; ls; cd -'
```

Remove the alias with the `unalias` command.

```
$ unalias foo
$ foo
bash: foo: command not found
```

Remember a moment ago when I said don't overwrite existing commands? Well there are actually situations where you might want to do this. For example, if you always use a particular option with a command you could create an alias by the same name but have it use the option you are so fond of. If you want your `ls` command to output files with a colour scheme then apply the `-G` option.

```bash
$ alias ls='ls -G'
$ type ls
ls is aliased to `ls -G'
```

The alias' you set up this way will not persist accross shell sessions, try exiting the shell and starting it up again, `type ls` will no longer be an alias. Setting up your favourite alias each time you open the shell would be a waste of time so later we will learn how to make them persist.

## Tasty Redirects

I/O stands for Input/Output, you can use it to redirect outputs to be the input of another command. Many commands, `ls` for example, output their results to a special file called _standard output_ (_stdout_) while their status message is output to _standard error_ (stderr_). Both stdout and stderr are displayed on the screen, not saved to a disk file. Finally there is _standard input_ (_stdin_) which comes from the keyboard. With I/O redirection we can change where outputs go to.

We'll start by redirecting an output from stdout to a new text file. 

```
$ ls -l /usr/bin > ls-output.txt
```

Now you'll have a text file in your working directory containing the result you would expect from `ls -l /usr/bin`.

What happens if the output is an error? Try this,

```
$ ls -l /bin/usr > ls-output.txt
ls: /bin/usr: No such file or directory
```

It seems that rather than sending the output to the file `ls-output.txt` it has been displayed for us. Remeber that there are two forms of output, stdout and stderr, there aren't any prizes for guessing which one the error is sent to but what do you think was sent to stdout? 

```
$ ls -l ls-output.txt
-rw-r--r--  1 me  staff  0 17 Mar 20:19 ls-output.txt
```

The file is now empty! This happens because destination files for redirected outputs are rewritten at the start, so if there is no output then we end up with an emty file. Use this to create an empty file whenever you want by omitting the command preceding the redirect, like this `> empty-file.txt`.

Append a file rather than overwrite it by doubling up on the arrows.

```
$ ls -l /usr/bin >> ls-output.txt
```

What about that error earlier, you want to redirect it? The shell references stdin, stdout and stderr as file streams 0, 1 and 2 respectively. These can be referred to when redirecting outputs.

```
$ ls -l /bin/usr 2> ls-error.txt
```

Check out the `ls-error.txt` file, you'll find your error message happily waiting for you. What if you want it in the same file as the stdouput stream?

```
$ ls -l /bin/usr &> ls-output.txt
```

On older format for this was `ls -l /bin/usr > ls-output.txt 2>&1`.

There is a deep dark hole where you can throw outputs you never want to see or hear about, a place called `/dev/null`, also known as a 'bit bucket', redirect outputs to there and it will be like they never existed. 

Time to learn about redirecting stdin. The `cat` command can read and output files like so

```
$ cat ls-output.txt

total 104304
-rwxr-xr-x   4 root   wheel       925 18 Aug  2018 2to3-
lrwxr-xr-x   1 root   wheel        74 14 Jan 14:06 2to3-2.7 -> ../../System/Library/Frameworks/Python.framework/Versions/2.7/bin/2to3-2.7
-rwxr-xr-x   1 root   wheel     55072 30 Nov 05:55 AssetCacheLocatorUtil
-rwxr-xr-x   1 root   wheel     53472 30 Nov 05:55 AssetCacheManagerUtil
-rwxr-xr-x   1 root   wheel     48256 30 Nov 05:55 AssetCacheTetheratorUtil
-rwxr-xr-x   1 root   wheel     18320  5 Feb 05:28 BuildStrings
-rwxr-xr-x   1 root   wheel     18288  5 Feb 05:28 CpMac
-rwxr-xr-x   1 root   wheel     18288  5 Feb 05:28 DeRez
-rwxr-xr-x   1 root   wheel     18320  5 Feb 05:28 GetFileInfo
-rwxr-xr-x   1 root   wheel     78000  5 Feb 05:27 IOAccelMemory
...
```

`cat` can also be used to join files, for example files split and named like so `movie.mpeg.001, movie.mpeg.002, movie.mpeg.003 ...` can be joined.

```
$ cat movie.mpeg.0* > movie.mpeg`
```

Wildcards expand in sorted order so your movie should make snese when you watch it. We're getting to the input redirection now, what happens if you use the cat command without any arguments? Give it a go. Nothing happened? Well you aren't being prompted for another command so what are we supposed to do? You console is actually waiting for some input, try typing some text. When you hit `CTRL-D` to tell `cat` we're at the _end of file_ (EOF) for the standard input you will notice the text is echoed back. 

Now we can see how the command below is a redirection of stdin from the keyboard to the file named _lazy_dog.txt_.

```
$ cat < lazy_dog.txt
the quick brown fox jumps over the lazy dog.
```

This isn't a particularly exciting example of input redirection, let's hope something more fun turns up later.

## Pipe up

Pipes (`|`) are a shell feature utilizing redirection of inputs and outputs. stdout of one command can be _piped_ into the stdin of another.

```
$ ls -l /usr/bin | less
```

Rather than saving the output to a file, this will output the stdout of the `ls` command to the stdin of less. Before, this would have taken two separate commands, `ls -l /usr/bin > ls-output.txt` followed by `less ls-output.txt`

A series of commands connected by pipes is referred to as a _pipeline_, these can make up some complex commands. There are particular commands utlised in pipelines to create filters for data, a simple example being `sort`.

```
$ ls /bin /usr/bin | sort | less
```

The command above lists all the files from `/bin` and `/usr/bin` in sorted order, rather than being split into their parent files. You could remove any duplicates by adding another filter, `uniq`.

```
$ ls /bin /usr/bin | sort | uniq | less
```

Rather than piping everything into the `less` command, let's try getting the word count. The `wc` command outputs the number of lines, words and bytes in a file, using the `-l` flag we can limit it to only output the number of lines, which in this case will be the total number of results.

```
$ ls /bin /usr/bin | sort | uniq | wc -l
1007
```

So we have 1007 items in `/bin` and `/usr/bin`, neat!

There are a couple more things to be thrown at you before we move on from piping. `grep` is a program for finding patterns, let's say you want to find all the results of our last command which contain the text 'zip'. The usual format for a grep command is `grep pattern file`, with pipes we can ignore the file input and instead pipe it in.

```
$ ls /bin /usr/bin | sort |uniq | grep zip
```

Return only the top or bottom 10 lines with the commands `head` and `tail`, adjust the number of lines printed with the `-n` option. Again, these command can be used on their own or in pipes. `tail` has a very useful option, `-f`, which allows you to view the file in real time. This is handy for viewing process logs as they are being written.

```
$ ls /bin /usr/bin | sort |uniq | head -n 5
```

Have a look at a log file in your system, the command below will display life process info for the wifi on your computer, try turning your wifi off and on to see the logs updating. Use _CTRL-C_ to exit.

```
$ tail -f /var/log/wifi.log
```

One last command for your piping needs, you can view the intermidiate stages of the data flowing through the pipeline by using the `tee` command. `tee` copies the stdin to stdout and any files specified.

```
$ ls /usr/bin | tee ls.txt | grep zip
```