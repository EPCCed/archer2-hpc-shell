---
title: "Working with files"
teaching: 30
exercises: 15
---



:::::: questions
  - How do I create/edit text files?
  - How do I move/copy/delete files?
::::::

:::::: objectives
  - Learn to use the `nano` text editor.
  - Understand how to move, create, and delete files.
::::::

Now that we know how to move around and look at things, let's learn how to
read, write, and handle files! We'll start by moving back to our home directory
and creating a scratch directory:

```bash
$ cd ~
$ mkdir hpc-test
$ cd hpc-test
```


## Creating and Editing Text Files

When working on an HPC system, we will frequently need to create or edit text
files. Text is one of the simplest computer file formats, defined as a simple
sequence of text lines.

What if we want to make a file? There are a few ways of doing this, the easiest
of which is simply using a text editor. For this lesson, we are going to us
`nano`, since it's more intuitive than many other terminal text editors.

To create or edit a file, type `nano <filename>`, on the terminal, where
`<filename>` is the name of the file. If the file does not already exist, it
will be created. Let's make a new file now, type whatever you want in it, and
save it.

```bash
$ nano draft.txt
```

![Nano in action](fig/nano-screenshot.png)

Nano defines a number of *shortcut keys* (prefixed by the <kbd>Control</kbd> or
<kbd>Ctrl</kbd> key) to perform actions such as saving the file or exiting the
editor. Here are the shortcut keys for a few common actions:

* <kbd>Ctrl</kbd>+<kbd>O</kbd> &mdash; save the file (into a current name or a
  new name).

* <kbd>Ctrl</kbd>+<kbd>X</kbd> &mdash; exit the editor. If you have not saved
  your file upon exiting, `nano` will ask you if you want to save.

* <kbd>Ctrl</kbd>+<kbd>K</kbd> &mdash; cut ("kill") a text line. This command
  deletes a line and saves it on a clipboard. If repeated multiple times
  without any interruption (key typing or cursor movement), it will cut a chunk
  of text lines.

* <kbd>Ctrl</kbd>+<kbd>U</kbd> &mdash; paste the cut text line (or lines). This
  command can be repeated to paste the same text elsewhere.

::: callout
# Using `vim` as a text editor

From time to time, you may encounter the `vim` text editor. Although `vim`
isn't the easiest or most user-friendly of text editors, you'll be able to
find it on any system and it has many more features than `nano`.

`vim` has several modes, a "command" mode (for doing big operations, like
saving and quitting) and an "insert" mode. You can switch to insert mode with
the <kbd>i</kbd> key, and command mode with <kbd>Esc</kbd>.

In insert mode, you can type more or less normally. In command mode there are
a few commands you should be aware of:

 * `:q!` &mdash; quit, without saving
 * `:wq` &mdash; save and quit
 * `dd` &mdash; cut/delete a line
 * `y` &mdash; paste a line
:::

Do a quick check to confirm our file was created.

```bash
$ ls
```
```output
draft.txt
```

## Reading Files

Let's read the file we just created now. There are a few different ways of
doing this, one of which is reading the entire file with `cat`.

```bash
$ cat draft.txt
```
```output
It's not "publish or perish" any more,
it's "share and thrive".
```

By default, `cat` prints out the content of the given file. Although `cat` may
not seem like an intuitive command with which to read files, it stands for
"concatenate". Giving it multiple file names will print out the contents of the
input files in the order specified in the `cat`'s invocation. For example,

```bash
$ cat draft.txt draft.txt
```
```output
It's not "publish or perish" any more,
it's "share and thrive".
It's not "publish or perish" any more,
it's "share and thrive".
```

::: challenge
# Reading Multiple Text Files

Create two more files using `nano`, giving them different names such as
`chap1.txt` and `chap2.txt`. Then use a single `cat` command to read and
print the contents of `draft.txt`, `chap1.txt`, and `chap2.txt`.
:::

## Creating Directory

We've successfully created a file. What about a directory? We've actually done
this before, using `mkdir`.

```bash
$ mkdir files
$ ls
```
```output
draft.txt  files
```

## Moving, Renaming, Copying Files

**Moving** &mdash; We will move `draft.txt` to the `files` directory with `mv`
("move") command. The same syntax works for both files and directories: `mv <file/directory> <new-location>`

```bash
$ mv draft.txt files
$ cd files
$ ls
```
```output
draft.txt
```

**Renaming** &mdash; `draft.txt` isn't a very descriptive name. How do we go
about changing it? It turns out that `mv` is also used to rename files and
directories. Although this may not seem intuitive at first, think of it as
*moving* a file to be stored under a different name. The syntax is quite
similar to moving files: `mv oldName newName`.

```bash
$ mv draft.txt newname.testfile
$ ls
```
```output
newname.testfile
```

## Removing files

We've begun to clutter up our workspace with all of the directories and files
we've been making. Let's learn how to get rid of them. One important note
before we start... **when you delete a file on UNIX systems, they are gone
*forever***. There is no "recycle bin" or "trash". Once a file is deleted, it
is gone, never to return. So be *very* careful when deleting files.

Files are deleted with `rm file [moreFiles]`. To delete the `newname.testfile`
in our current directory:

```bash
$ ls
$ rm newname.testfile
$ ls
```
```output
files Documents newname.testfile
files Documents
```

That was simple enough. Directories are deleted in a similar manner using 
`rm -r` (the `-r` option stands for 'recursive').

```bash
$ ls
$ rm -r Documents
$ rm -r files
$ ls
```
```output
files Documents
rmdir: failed to remove `files/': Directory not empty
files
```

What happened? As it turns out, `rmdir` is unable to remove directories that
have stuff in them. To delete a directory and everything inside it, we will use
a special variant of `rm`, `rm -rf directory`. This is probably the scariest
command on UNIX- it will force delete a directory and all of its contents
without prompting. **ALWAYS** double check your typing before using it... if
you leave out the arguments, it will attempt to delete everything on your file
system that you have permission to delete. So when deleting directories be
very, very careful.

::: callout

# What happens when you use `rm -rf` accidentally

Steam is a major online sales platform for PC video games with over 125
million users. Despite this, it hasn't always had the most stable or
error-free code.

In January 2015, user kevyin on GitHub [reported that Steam's Linux client
had deleted every file on his
computer](https://github.com/ValveSoftware/steam-for-linux/issues/3671). It
turned out that one of the Steam programmers had added the following line:
`rm -rf "$STEAMROOT/"*`. Due to the way that Steam was set up, the variable
`$STEAMROOT` was never initialized, meaning the statement evaluated to `rm -rf /*`. 
This coding error in the Linux client meant that Steam deleted every
single file on a computer when run in certain scenarios (including connected
external hard drives). Moral of the story: **be very careful** when using `rm -rf`!
:::

## Looking at files

Sometimes it's not practical to read an entire file with `cat`- the file might
be way too large, take a long time to open, or maybe we want to only look at a
certain part of the file. As an example, we are going to look at a large and
complex file type used in bioinformatics- a `.gtf` file. The GTF2 format is
commonly used to describe the location of genetic features in a genome.

Let's grab and unpack a set of demo files for use later. To do this, we'll use
[`wget`](https://www.gnu.org/software/wget/) (`wget <link>` downloads a file from
a link).

```bash
$ wget https://epcced.github.io/2024-10-30-hpc-shell-edinburgh/files/bash-lesson.tar.gz
```

::: callout

# Problems with `wget`?

`wget` is a stand-alone application for downloading things over HTTP/HTTPS
and FTP/FTPS connections, and it does the job admirably &mdash; when it is
installed.

Some operating systems instead come with [cURL]( https://curl.haxx.se/),
which is the command-line interface to `libcurl`, a powerful library for
programming interactions with remote resources over a wide variety of network
protocols. If you have `curl` but not `wget`, then try this command instead:

```bash
$ curl -O https://epcced.github.io/2024-10-30-hpc-shell-edinburgh/files/bash-lesson.tar.gz
```

For very large downloads, you might consider using
[Aria2](https://aria2.github.io/), which has support for downloading the same
file from multiple mirrors. You have to install it separately, but if you
have it, try this to get it faster than your neighbors:

```bash
$ aria2c https://epcced.github.io/2024-10-30-hpc-shell-edinburgh/files/bash-lesson.tar.gz
```

:::


::: solution

# Install cURL

- macOS: `curl` is pre-installed on macOS. If you must have the latest
  version you can `brew install` it, but only do so if the stock version
  has failed you.

- Windows: `curl` comes preinstalled for the Windows 10 command line. For
  earlier Windows systems, you can download the executable
  [directly](https://curl.haxx.se/windows/); run it in place.
  `curl` comes preinstalled in [Git for Windows](https://gitforwindows.org/) and [Windows Subsystem for Linux](https://docs.microsoft.com/en-us/windows/wsl/install-win10). On
  [Cygwin](https://www.cygwin.com/), run the setup program again and
  select the `curl` package to install it.

- Linux: `curl` is packaged for every major distribution. You can install
  it through the usual means.
    - Debian, Ubuntu, Mint: `sudo apt install curl`
    - CentOS, Red Hat: `sudo yum install curl` or `zypper install curl`
    - Fedora: `sudo dnf install curl`

:::
::: solution
# Install Aria2

- macOS: `aria2c` is available through a homebrew. `brew install aria2`.

- Windows: download the latest [release](https://github.com/aria2/aria2/releases) and run `aria2c` in
  place. If you're using the [Windows Subsystem for Linux](https://docs.microsoft.com/en-us/windows/wsl/install-win10),

- Linux: every major distribution has an `aria2` package. Install it by the
  usual means.

    - Debian, Ubuntu, Mint: `sudo apt install aria2`
    - CentOS, Red Hat: `sudo yum install aria2` or `zypper install aria2`
    - Fedora: `sudo dnf install aria2`

:::

You'll commonly encounter `.tar.gz` archives while working in UNIX. To extract
the files from a `.tar.gz` file, we run the command `tar -xvf filename.tar.gz`:

```bash
$ tar -xvf bash-lesson.tar.gz
```
```output
dmel-all-r6.19.gtf
dmel_unique_protein_isoforms_fb_2016_01.tsv
gene_association.fb
SRR307023_1.fastq
SRR307023_2.fastq
SRR307024_1.fastq
SRR307024_2.fastq
SRR307025_1.fastq
SRR307025_2.fastq
SRR307026_1.fastq
SRR307026_2.fastq
SRR307027_1.fastq
SRR307027_2.fastq
SRR307028_1.fastq
SRR307028_2.fastq
SRR307029_1.fastq
SRR307029_2.fastq
SRR307030_1.fastq
SRR307030_2.fastq
```

::: callout
# Unzipping files

We just unzipped a .tar.gz file for this example. What if we run into other
file formats that we need to unzip? Just use the handy reference below:

 * `gunzip` extracts the contents of .gz files
 * `unzip` extracts the contents of .zip files
 * `tar -xvf` extracts the contents of .tar.gz and .tar.bz2 files
:::

That is a lot of files! One of these files, `dmel-all-r6.19.gtf` is extremely
large, and contains every annotated feature in the *Drosophila melanogaster*
genome. It's a huge file- what happens if we run `cat` on it? (Press <kbd>Ctrl + c</kbd>
to stop it).

So, `cat` is a really bad option when reading big files... it scrolls through
the entire file far too quickly! What are the alternatives? Try all of these
out and see which ones you like best!

- `head <file>`: Print the top 10 lines in a file to the console. You can control
  the number of lines you see with the `-n <numberOfLines>` flag.
- `tail <file>`: Same as `head`, but prints the last 10 lines in a file to the
  console.
- `less <file>`: Opens a file and display as much as possible on-screen. You can
  scroll with <kbd>Enter</kbd> or the arrow keys on your keyboard. Press <kbd>q</kbd> to close
  the viewer.

::: challenge
Out of `cat`, `head`, `tail`, and `less`, which method of reading files is your
favourite? Why?
:::

:::::: keypoints
- Use `nano` to create or edit text files from a terminal.
- Use `cat file1 [file2 ...]` to print the contents of one or more files to
  the terminal.
- Use `mv old dir` to move a file or directory `old` to another directory `dir`.
- Use `mv old new` to rename a file or directory `old` to a `new` name.
- Use `cp old new` to copy a file under a new name or location.
- Use `cp old dir` copies a file `old` into a directory `dir`.
- Use `rm old` to delete (remove) a file.
- File extensions are entirely arbitrary on UNIX systems.
::::::
