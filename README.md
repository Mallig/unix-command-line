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

### What do your `-l`f Eyes See?

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

Hard links cannot reference a file outside its own file system nor can it reference a directory. They are indistinguishable from the file itself and directory lists will give no special indication of its presence. When a hard link is deleted the contents of the file will remain until all links to the file are deleted. Symbolic links are newer and preferred.

Symbolic links don't have the limitations of hard links, they work by creating a file with a text pointer to the referenced file/directory. The symbolic link and the file it points to are almost indistinguishable, writing something to a symbolic link will also write it to the referenced file. Deleting a symbolic link will not delete the referenced file, deleting the file will not selete the link but the link will now point to nothing (broken link).

Alright, that may have been a bit too much info so lets start experimenting.