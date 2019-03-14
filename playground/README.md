## All Work and no Play makes me Forget what I Learned

I'm going to run through a little recap of what we've learned to try and solidify the concepts in our minds. 

Lets start by giving ourselves something to look at.

```
mkdir dir1 dir2
```

Copy some info into the directory (I'm pretty sure you're safe to have this info public, but I wouldn't recommend it just in case)

```
cp /etc/passwd .
```

The single dot here represents the working directory, /etc/passwd is an absolute path. Get a quick look at what we currently have.

```
ls -l

total 24
-rw-r--r--  1 me  staff   246 14 Mar 16:06 README.md
drwxr-xr-x  2 me  staff    64 14 Mar 15:58 dir1
drwxr-xr-x  2 me  staff    64 14 Mar 15:59 dir2
-rw-r--r--  1 me  staff  6804 14 Mar 16:07 passwd
```

Let's copy again with the -v option (remember you can just press up to go through your command history)

```
cp -v /etc/passwd .

/etc/passwd -> ./passwd
```

We got a nice little message informing us what happened, we also just overwrote the old file. Let's go again but make the system prompt us this time.

```
cp -i /etc/passwd .
overwrite ./passwd? (y/n [n]) 
```
Hit `y` to continue with the copy command and overwite the original file.

Change the name of the file, passwd is a bit boring

```
mv passwd fun
```

Now lets put it in one fo the directories

```
mv fun dir1
```

I changed my mind, lets put it in the other one

```
mv dir1/fun dir2
```

Actually, it should stay in out of both

```
mv dir2/fun .
```

No, leave it in dir1

```
mv fun dir1
```

And let's move some of the directories about now

```
mv dir1 dir2
```

If dir2 had not existed that command would have renamed dir1 to dir2

Ok let's move everything back to where it began.

```
mv dir2/dir1 .
mv dir1/fun .
```

Time to make some links, start with the hard stuff.

```
ln fun fun-hard
ln fun dir1/fun-hard
ln fun dir2/fun-hard
```

How can we tell that fun and the fun-hard links are actually the same file? We can see that they are the same size but that's not a guarantee they are the same. All files are made up of two parts, a data part and a name part. Hard links are additional name parts referring to the same data part. Unix systems "assign a chain of disk blocks to what is called an inode, which is then associated with the name part". Hard links refer to these inodes and they can be revealed with the `-i` option for the `ls` command.

```
ls -i

4099952 -rw-r--r--  1 malachygilchrist  staff  1597 14 Mar 16:34 README.md
4101739 drwxr-xr-x  3 malachygilchrist  staff    96 14 Mar 16:41 dir1
4101771 drwxr-xr-x  3 malachygilchrist  staff    96 14 Mar 16:42 dir2
4102624 -rw-r--r--  4 malachygilchrist  staff  6804 14 Mar 16:12 fun
4102624 -rw-r--r--  4 malachygilchrist  staff  6804 14 Mar 16:12 fun-hard
```

The first column refers to the inode number and as you can see fun and fun-hard are the same.

Time for some symbolic links.

```
ln -s fun fun-sym
ln -s ../fun dir1/fun-sym
ln -s ../fun dir2/fun-sym
```

The first command is easy to understand, you just provided the `-s` option to create a symbolic link rather than hard. However what are the other two commands doing? Symbolic links are "descriptions of where the target file is relative to the symbolic link", it may be more obvious if you look for yourself.

```
ls -l dir1

total 16
-rw-r--r--  4 malachygilchrist  staff  6804 14 Mar 16:12 fun-hard
lrwxr-xr-x  1 malachygilchrist  staff     6 14 Mar 17:20 fun-sym -> ../fun
```

The response is nice enough to tell us that fun-sym points towards fun in the parent directory, no messing around with inodes here (it wouldn't work anyway, symbolic links have their own inodes). You can use absolute pathnames rather than the relative pathname used here, however relative is preferred as it's less likely to lead to broken links.

Lets make a sybolic link to a directory

```
ln -s dir1 dir1-sym
```

Time to start deleting things, let's start with the hard link in the working directory.

```
rm fun-hard
```

Did you notice that the link count for fun dropped by one? Let's get rid of fun completely (and see it happening).

```
rm -i fun
remove fun?
```

Press y and enter to continue to a life devoid of fun. The symbolic link to fun still exists though! What happens if we try and use it?

```
less fun-sym
fun-sym: No such file or directory
```

It's a broken link, not the end of the world but it's best not to keep them around. Something to remember is that when performing operations on symbolic links the operation is actually carried out on the file it refers to, this is not the case for `rm`. 

Let's leave the playground how we found it, delete the contents but leave the README so you can try this again later.

```
rm -r fun* dir*
```

Try this playground if you need refreshing on the basic commands, hope it's been helpful :)

