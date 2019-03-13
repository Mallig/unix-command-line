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