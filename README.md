# Exercise for learning STDERR and STDIN, and working on a remote server

- Last modified: mån feb 05, 2024  04:51
- Sign: Johan.Nylander
- Please see the [Setup](#setup) in the end for installation and requirements.

---

## Task

Start program [`prog`](prog) on remote server and handle output.

## Background

You want to start a time-consuming analysis on a remote server, but you
do not have time so sit and wait for the process to finish, so you want
to be able to logout and check in again later to fetch the results.

## Outline of workflow

1. Connect to server
2. Create a new directory for the exercise
3. Start the job
4. Logout from the server (while keeping your program running)
5. Login again
6. Terminate (kill) the running process
7. Create a compressed archive (`*.tgz`) containing the result
8. Transfer the compressed archive to your local computer
9. Uncompress the archive locally

## Steps

### 1. Use ssh to connect to BBB

First, make sure you are connected to the local (course) WiFi network,
otherwise the server can't be found (see [Setup](#setup) below).

The way you connect with `ssh` is:

    $ ssh usernameonserver@addresstoserver

The `addresstoserver` is the IP number or name of BBB. The
`usernameonserver` is the user name you was given on the BBB server.

You will be asked if you really want to connect to this server, and then be
asked for a password.  The password is the one associated with the
`usernameonserver`.

Note: If/when you want to disconnect from the remote server, type `exit`.

### 2. Create a new directory for the exercise

Remember `mkdir` command?

### 3. Start the program (named `prog`) and handle the output

You want to make sure you save *the very important results* from `prog`.  You
also want to make sure you can logout while your program `prog` is running.
How can this be achieved?

**Hint:** Do you remember STDERR and STDOUT?

Useful tools are the redirection operators (`<`, `>`, `2>`, `>>`, `2>>`) and
the use of `&` to put processes in the background.

Examples:

    # Redirect stdout to newfile. newfile will be created if it's not there,
    # otherwise owerwritten.
    $ cat infile > newfile

    # Redirect stdout and append to newfile. newfile will be created if it's
    # not there.
    $ cat infile >> newfile

    # Redirect stdin
    $ someprogram < infile

    # Redirect stderr. newfile will be created if it's not there, otherwise
    # owerwritten.
    $ cat infile 2> newfile

    # Redirection can be combined.
    $ cat infile > newfile1 2> newfile2
    $ someprogram < infile > newfile1 2> newfile2

    # Put process in the background (i.e., "get the prompt back so we can
    # logout")
    $ someprogram &

    # And all at once (very useful). Here, `someprogram` reads input from a file.
    $ someprogram infile > outfile 2> logfile &

### 4. Make sure `prog` is running in the background (Step 3.), then logout from the server

To logout, type `exit`

### 5. Log back in again, and check the progress of your job

Useful tools: `ps`, `tail`, `top -c` (type `q` to exit `top`).

### 6. Terminate (kill) your running job

First you need to find the process ID of your job. This can be seen by using
`ps` or `top` (press `q` to exit `top`).  Then you can kill the process by
using, e.g., `kill` or `killall`.  `kill` uses the process ID, and `killall`
the name of the program as argument.

Examples:

    $ kill 11296
    $ killall prog

### 7. Gather your very important results

A good way to do this (and to get faster file transfer) is to use `tar`.

Remember the `tar` syntax?

### 8. Transfer the file(s) back to your computer!

This is done by using secure copy (`scp`).

General syntax:

\small

    $ scp useronmachine1@machine1:/path/to/sourcefile useronmachine2@machine2:/path/to/destinationfile

\normalsize

Start by making sure you know all the relevant information on paths, users,
machines etc.  The command can be issued from any computer, that is, in this
example you can issue the command in a terminal connected to the server, or in
a terminal connected on your local machine. Furthermore, you can take advantage
of the fact that `scp` have some default settings, which allows you to use
relative paths etc.

##### Example 1: fetching a remote file from server to your local computer (where you are typing the command):

    $ scp useronremoteserver@remoteserver:/path/to/sourcefile .

`useronremoteserver` might be `user01`, and `remoteserver` might be `10.0.0.1`.
Note the period (`.`) in the end of the command. This stands for "right here"
and is the current working directory, i.e., the directory from where the `scp`
command is issued.

##### Example 2: Copying a file from remote server (where you are typing the command) to your local computer:

    $ scp sourcefile useronlocalcomputer@localcomputer:.

`useronlocalcomputer` -- type `whoami` in a terminal on the local computer if
you don't remember who you are!

`localcomputer` -- you can see the IP address of your local computer by typing
`hostname -I`.

Note the period (`.`) at the end of the command. This stands for "right here",
and in the current context it will be the home directory of
`useronlocalcomputer` on the local computer.

As a note, if you can make sure to have the same user name on local and remote
servers, the `useronremoteserver@` part can be left out. Furthermore, if you
set up your `ssh` connection with keys, then there is no prompting for
passwords (see, e.g.,
<http://yourtoolbox.blogspot.se/2012/08/ssh-connection-with-no-password.html>)

### 9. Uncompress locally

Remember the arguments to `tar`?

### 10. Extra: Now repeat all of this, but fetch the output from the progress printing!

---

## Setup

This exercise requires a server (BBB) where users can log in and start the
pre-installed program [`prog`](prog). The server should allow users to run
processes in the background.  The first version of this exercise used the
server setup described here: <https://github.com/nylander/BBB>.

The program [`prog`](prog) is written in Perl and requires `perl` (invoked by
`/usr/bin/env perl`).

Target audience are beginners in using the command line for computing.

Linux commands and operators used in this exercise: `cd`, `ls`, `ssh`, `mkdir`,
`<`, `>`, `2>`, `>>`, `2>>`, `&`, `cat`, `exit`, `ps`, `tail`, `top`, `kill`,
`killall`, `tar`, `scp`, `hostname`, `whoami`, `STDERR`, `STDOUT`, `Ctrl+c`.

