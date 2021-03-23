# MANAGE SERVER WITH LINUX
## Table of content:
- [Introduction](#introduction)
- [Bash Shell Command](#bash-shell-command)
- [Processes Management](#processes-management)
- [System Resources](#system-resources)
- [Installation Packages Management](#installation-packages-management)
- [Log Server Management](#log-server-management)
- [Console](#console)
- [Network and IPTables](#network-and-iptables)
- [Web Server Installation](#web-server-installation)
- [Services and Network in Host](#services-and-network-in-host)
- [Other skills](#other-skills)
- [References](#references)

***

## *Introduction*
<details><summary>Linux Shell and Shell Scripting</summary>

### Reference link:

* [Shell script bash file](https://www.geeksforgeeks.org/introduction-linux-shell-shell-scripting/)


If you are using any major operating system you are indirectly interacting to shell. If you are running Ubuntu, Linux Mint or any other Linux distribution, you are interacting to shell every time you use terminal. In this article I will discuss about linux shells and shell scripting so before understanding shell scripting we have to get familiar with following terminologies:

* Kernel
* Shell
* Terminal

### What is Kernel?

The kernel is a computer program that is the core of a computer's operating system, with complete control over everthing in the system. It manages following resources of the Linux system - 

* File management
* Process management
* I/O management
* Memory management
* Device management etc.

### What is Shell?

A shell is special user program which provide  an interface to user to use operating system services. Shell accept human readble commands from user and convert them into something which kernel can such as keyboards or from files. The shell gets started when the user logs in or start the terminal.

![linux shell](https://media.geeksforgeeks.org/wp-content/uploads/18834419_1198504446945937_35839918_n-300x291.png)

Shell is broadly classified into two categories -

* Command Line Shell
* Graphical Shell

### Command Line Shell

Shell can be accessed by user using a command line interface. A special program called Terminal in linux/macOS or Command Prompt in Windows OS is provided to type in the human readable commands such as “cat”, “ls” etc. and then it is being execute. The result is then displayed on the terminal to the user. A terminal in Ubuntu 16.4 system looks like this –

![linux command line](https://media.geeksforgeeks.org/wp-content/uploads/cli_example.png)

In above screenshot “ls” command with “-l” option is executed.
It will list all the files in current working directory in long listing format.
Working with command line shell is bit difficult for the beginners because it’s hard to memorize so many commands. It is very powerful, it allows user to store commands in a file and execute them together. This way any repetitive task can be easily automated. These files are usually called batch files in Windows and Shell Scripts in Linux/macOS systems.

### Graphical Shells

Graphical shells provide means for manipulating programs based on graphical user interface (GUI), by allowing for operations such as opening, closing, moving and resizing windows, as well as switching focus between windows. Window OS or Ubuntu OS can be considered as good example which provide GUI to user for interacting with program. User do not need to type in command for every actions.A typical GUI in Ubuntu system –

[GUI shell](https://media.geeksforgeeks.org/wp-content/uploads/GUI-shell.png)

There are several shells are available for Linux systems like –

* BASH (Bourne Again SHell) – It is most widely used shell in Linux systems. It is used as default login shell in Linux systems and in macOS. It can also be installed on Windows OS.
* CSH (C SHell) – The C shell’s syntax and usage are very similar to the C programming language.
* KSH (Korn SHell) – The Korn Shell also was the base for the POSIX Shell standard specifications etc.

Each shell does the same job but understand different commands and provide different built in functions.

### Shell scripting

  Usually shells are interactive that mean, they accept command as input from users and execute them. However some time we want to execute a bunch of commands routinely, so we have type in all commands each time in terminal.
As shell can also take commands as input from file we can write these commands in a file and can execute them in shell to avoid this repetitive work. These files are called Shell Scripts or Shell Programs. Shell scripts are similar to the batch file in MS-DOS. Each shell script is saved with .sh file extension eg. myscript.sh
  A shell script have syntax just like any other programming language. If you have any prior experience with any programming language like Python, C/C++ etc. it would be very easy to get started with it.
A shell script comprises following elements –

* Shell Keywords – if, else, break etc.
* Shell commands – cd, ls, echo, pwd, touch etc.
* Functions
* Control flow – if..then..else, case and shell loops etc.

### Why do we need shell scripts

There are many reasons to write shell scripts –

* To avoid repetitive work and automation
* System admins use shell scripting for routine backups
* System monitoring
* Adding new functionality to the shell etc.

### Advantages of shell scripts

* The command and syntax are exactly the same as those directly entered in command line, so programmer do not need to switch to entirely different syntax
* Writing shell scripts are much quicker
* Quick start
* Interactive debugging etc.

### Disadvantages of shell scripts

* Prone to costly errors, a single mistake can change the command which might be harmful
* Slow execution speed
* Design flaws within the language syntax or implementation
* Not well suited for large and complex task
* Provide minimal data structure unlike other scripting languages. etc

### Simple demo of shell scripting using Bash Shell

If you work on terminal, something you traverse deep down in directories. Then for coming few directories up in path we have to execute command like this as shown below to get the "python" directory -

[](https://media.geeksforgeeks.org/wp-content/uploads/old_way_cd.png)

It is quite frustrating, so why not we can have a utility where we just have to type the name of directory and we can directly jump to that without executing “cd ../” command again and again. Save the script as “jump.sh”

    # !/bin/bash 

    # A simple bash script to move up to desired directory level directly 

    function jump() 
    { 
      # original value of Internal Field Separator 
      OLDIFS=$IFS 

      # setting field separator to "/"  
      IFS=/ 

      # converting working path into array of directories in path 
      # eg. /my/path/is/like/this 
      # into [, my, path, is, like, this] 
      path_arr=($PWD) 

      # setting IFS to original value 
      IFS=$OLDIFS 

      local pos=-1 

      # ${path_arr[@]} gives all the values in path_arr 
      for dir in "${path_arr[@]}"
      do
          # find the number of directories to move up to 
          # reach at target directory  
          pos=$[$pos+1] 
          if [ "$1" = "$dir" ];then

              # length of the path_arr 
              dir_in_path=${#path_arr[@]} 

              #current working directory 
              cwd=$PWD 
              limit=$[$dir_in_path-$pos-1] 
              for ((i=0; i<limit; i++)) 
              do
                  cwd=$cwd/.. 
              done
              cd $cwd 
              break
          fi
      done
    }

For now we cannot execute our shell script because it do not have permissions. We have to make it executable by typing following command -

    $ chmod -x path/to/our/file/jump.sh

Now to make this available on every terminal session, we have to put this in ".bashrc" file. “.bashrc” is a shell script that Bash shell runs whenever it is started interactively. The purpose of a .bashrc file is to provide a place where you can set up variables, functions and aliases, define our prompt and define other settings that we want to use whenever we open a new terminal window.
Now open terminal and type following command –

    $ echo “source ~/path/to/our/file/jump.sh”>> ~/.bashrc

Now open  your terminal and try out new "jump" functionality by typing following command -

    $ jump dir_name

just like below screenshot -

> **Resources for learning Bash Scripting** 

* https://bash.cyberciti.biz/guide/The_bash_shell
* http://tldp.org/LDP/abs/html/

> **References**

* https://en.wikipedia.org/wiki/Shell_script
* https://en.wikipedia.org/wiki/Shell_(computing)

This article is contributed by Atul Kumar. If you like GeeksforGeeks and would like to contribute, you can also write an article using contribute.geeksforgeeks.org or mail your article to contribute@geeksforgeeks.org. See your article appearing on the GeeksforGeeks main page and help other Geeks.Please write comments if you find anything incorrect, or you want to share more information about the topic discussed above.

Attention reader! Don’t stop learning now. Get hold of all the important CS Theory concepts for SDE interviews with the CS Theory Course at a student-friendly price and become industry ready.

[](https://media.geeksforgeeks.org/wp-content/uploads/jump_way_cd-1.png)

> REFERENCE:  https://www.geeksforgeeks.org/introduction-linux-shell-shell-scripting/

</details>

## *Bash Shell Command*
<details>
<summary>System role and definition commands</summary>

 - ##ls
    
    ###Examples:

    * List files using ls with no option
    
      **ls** with no option list files and directories in bare format where we won’t be able to view details like file types, size, modified date and time, permission and links etc.
       ```
       buichidung@CPU002169:~/Documents/Studying$ ls
      awesome-python  Git  gittemp  Project_temp  root  Studying
       ```
    * List files with option -l

      Here, **ls -l** (-l is character not one) shows file or directory, size, modified date and time, file or folder name and owner of file and its permission.

      ```
      buichidung@CPU002169:~/Documents/Studying$ ls -l
      total 20
      drwxrwxr-x 5 buichidung buichidung 4096 Thg 10 19 11:24 awesome-python
      drwxrwxr-x 3 buichidung buichidung 4096 Thg 10 20 14:14 Git
      -rw-rw-r-- 1 buichidung buichidung    0 Thg 10 19 11:17 gittemp
      drwxrwxr-x 3 buichidung buichidung 4096 Thg 10 20 07:32 Project_temp
      drwxrwxr-x 4 buichidung buichidung 4096 Thg 10 20 14:57 root
      drwxrwxr-x 3 buichidung buichidung 4096 Thg 10 19 11:27 Studying
      ```

    * View hidden files

      List all files including hidden file starting with '.'.

      ```
      buichidung@CPU002169:~/Documents/Studying$ ls -a
      .  ..  awesome-python  .git  Git  gittemp  Project_temp  root  Studying
      ```

    * List files with Human Readable Format with option -lh

      With combination of **-lh** option, shows sizes in human readable format.

      ```
      buichidung@CPU002169:~/Documents/Studying$ ls -lh
      total 20K
      drwxrwxr-x 5 buichidung buichidung 4,0K Thg 10 19 11:24 awesome-python
      drwxrwxr-x 3 buichidung buichidung 4,0K Thg 10 20 14:14 Git
      -rw-rw-r-- 1 buichidung buichidung    0 Thg 10 19 11:17 gittemp
      drwxrwxr-x 3 buichidung buichidung 4,0K Thg 10 20 07:32 Project_temp
      drwxrwxr-x 4 buichidung buichidung 4,0K Thg 10 20 14:57 root
      drwxrwxr-x 3 buichidung buichidung 4,0K Thg 10 19 11:27 Studying
      ```

    * List Files and Directories with '/' character at the end

      Using **-F** option with **ls** command, will add the '/' character at the end each directory

      ```
      buichidung@CPU002169:~/Documents/Studying$ ls -F
      awesome-python/  Git/  gittemp  Project_temp/  root/  Studying/
      ```

    * List files in reverse order

      The following command with **ls** -r option display files and irectories in reverse order

      ```
      buichidung@CPU002169:~/Documents/Studying$ ls -r
      Studying  root  Project_temp  gittemp  Git  awesome-python
      ```

    * Recursively list Sub-Directories

      **ls -R** option will list very long listing directory trees. See an example of output of the command.

      ```
      buichidung@CPU002169:~/Documents/Studying$ ls -R
      .:
      awesome-python  Git  gittemp  Project_temp  root  Studying

      ./awesome-python:
      CONTRIBUTING.md  LICENSE   mkdocs.yml  requirements.txt
      docs             Makefile  README.md   sort.py

      ./awesome-python/docs:
      CNAME  css

      ./awesome-python/docs/css:
      extra.css

      ./Git:
      main.py

      ./Project_temp:
      main.py

      ./root:
      constant.py  database.pyc  __pycache__  temp.txt
      database.py  main.py       student.py   util.py

      ./root/__pycache__:
      constant.cpython-38.pyc  student.cpython-38.pyc
      database.cpython-38.pyc  util.cpython-38.pyc

      ./Studying:
      ```

    * Reverse output order

      With combination of **-ltr** will shows lastest modification ifle or directory date as last

      ```
      buichidung@CPU002169:~/Documents/Studying$ ls -ltr
      total 20
      -rw-rw-r-- 1 buichidung buichidung    0 Thg 10 19 11:17 gittemp
      drwxrwxr-x 5 buichidung buichidung 4096 Thg 10 19 11:24 awesome-python
      drwxrwxr-x 3 buichidung buichidung 4096 Thg 10 19 11:27 Studying
      drwxrwxr-x 3 buichidung buichidung 4096 Thg 10 20 07:32 Project_temp
      drwxrwxr-x 3 buichidung buichidung 4096 Thg 10 20 14:14 Git
      drwxrwxr-x 4 buichidung buichidung 4096 Thg 10 20 14:57 root
      ```

    * Sort file by file size

      With combination of -lS displays file size in order, will display big in size first

      ```
      buichidung@CPU002169:~/Documents/Studying$ ls -lS
      total 20
      drwxrwxr-x 5 buichidung buichidung 4096 Thg 10 19 11:24 awesome-python
      drwxrwxr-x 3 buichidung buichidung 4096 Thg 10 20 14:14 Git
      drwxrwxr-x 3 buichidung buichidung 4096 Thg 10 20 07:32 Project_temp
      drwxrwxr-x 4 buichidung buichidung 4096 Thg 10 20 14:57 root
      drwxrwxr-x 3 buichidung buichidung 4096 Thg 10 19 11:27 Studying
      -rw-rw-r-- 1 buichidung buichidung    0 Thg 10 19 11:17 gittemp
      ```

    * Display inot number of File or Directory

      We can see some number printed before file / directory name. With **-i** options list file / directory with inode number

      ```
      buichidung@CPU002169:~/Documents/Studying$ ls -i
      39980431 awesome-python  39980427 gittemp       39980399 root
      39980378 Git             43123170 Project_temp  39980504 Studying
      ```

    * Shows version of ls command

      Check version of ls command

      ```
      buichidung@CPU002169:~/Documents/Studying$ ls --version
      ls (GNU coreutils) 8.30
      Copyright (C) 2018 Free Software Foundation, Inc.
      License GPLv3+: GNU GPL version 3 or later <https://gnu.org/licenses/gpl.html>.
      This is free software: you are free to change and redistribute it.
      There is NO WARRANTY, to the extent permitted by law.


      Written by Richard M. Stallman and David MacKenzie.
      ```

    * Show help page

      List help page of ls command with their option.

      ```
      buichidung@CPU002169:~/Documents/Studying$ ls --version
      ...
      ```

    * List Directory Information

      With **ls -l** command list files under directory **/tmp**. Wherein with **-ld** parameters displays information of **/tmp** directory.

      ```
      buichidung@CPU002169:~/Documents/Studying$ ls -l /tmp
      total 56
      -rw------- 1 buichidung buichidung    0 Thg 3  22 07:05 config-err-fsKJ2L
      drwxr-xr-x 2 buichidung buichidung 4096 Thg 3  22 07:16 hsperfdata_buichidung
      drwx------ 3 root       root       4096 Thg 3  22 07:05 snap.snap-store
      drwx------ 2 buichidung buichidung 4096 Thg 3  22 07:05 ssh-qJOOdh0gGdVG
      drwx------ 3 root       root       4096 Thg 3  22 06:51 systemd-private-1ab5795ef2be417781758e590d164531-colord.service-tzoq1f
      drwx------ 3 root       root       4096 Thg 3  22 06:50 systemd-private-1ab5795ef2be417781758e590d164531-ModemManager.service-F6wNIh
      drwx------ 3 root       root       4096 Thg 3  22 06:50 systemd-private-1ab5795ef2be417781758e590d164531-switcheroo-control.service-BNEE8g
      drwx------ 3 root       root       4096 Thg 3  22 06:50 systemd-private-1ab5795ef2be417781758e590d164531-systemd-logind.service-nvGLwh
      drwx------ 3 root       root       4096 Thg 3  22 06:50 systemd-private-1ab5795ef2be417781758e590d164531-systemd-resolved.service-dpgimj
      drwx------ 3 root       root       4096 Thg 3  22 06:50 systemd-private-1ab5795ef2be417781758e590d164531-systemd-timesyncd.service-kDUO6e
      drwx------ 3 root       root       4096 Thg 3  22 06:51 systemd-private-1ab5795ef2be417781758e590d164531-upower.service-AhPEhg
      -rw------- 1 buichidung buichidung    0 Thg 3  22 07:45 tpxUkxyz
      drwx------ 2 buichidung buichidung 4096 Thg 3  22 09:18 tracker-extract-files.1001
      drwx------ 2 gdm        gdm        4096 Thg 3  22 06:51 tracker-extract-files.125
      drwx------ 2 postgres   postgres   4096 Thg 3  22 07:16 tracker-extract-files.127
      drwx------ 4 buichidung buichidung 4096 Thg 3  22 07:45 vmware-buichidung
      ```

      ```
      buichidung@CPU002169:~/Documents/Studying$ ls -ld /tmp/
      drwxrwxrwt 22 root root 4096 Thg 3  22 13:48 /tmp/
      ```

    * Display UID and GID of Files

      To display **UID** and **GID** of files and directories. Use option -n with ls command

      ```
      buichidung@CPU002169:~/Documents/Studying$ ls -n
      total 20
      drwxrwxr-x 5 1001 1001 4096 Thg 10 19 11:24 awesome-python
      drwxrwxr-x 3 1001 1001 4096 Thg 10 20 14:14 Git
      -rw-rw-r-- 1 1001 1001    0 Thg 10 19 11:17 gittemp
      drwxrwxr-x 3 1001 1001 4096 Thg 10 20 07:32 Project_temp
      drwxrwxr-x 4 1001 1001 4096 Thg 10 20 14:57 root
      drwxrwxr-x 3 1001 1001 4096 Thg 10 19 11:27 Studying
      ```

    * ls command and its Aliases

      We have made alias for **ls** command, when we execute ls command it will take **-l** option by default and display long listing as mentioned earlier

      ```
      buichidung@CPU002169:~/Documents/Studying$ alias ls="ls -l"
      buichidung@CPU002169:~/Documents/Studying$ ls
      total 20
      drwxrwxr-x 5 buichidung buichidung 4096 Thg 10 19 11:24 awesome-python
      drwxrwxr-x 3 buichidung buichidung 4096 Thg 10 20 14:14 Git
      -rw-rw-r-- 1 buichidung buichidung    0 Thg 10 19 11:17 gittemp
      drwxrwxr-x 3 buichidung buichidung 4096 Thg 10 20 07:32 Project_temp
      drwxrwxr-x 4 buichidung buichidung 4096 Thg 10 20 14:57 root
      drwxrwxr-x 3 buichidung buichidung 4096 Thg 10 19 11:27 Studying
      ```
      To remove an alias previously defined, just use the unalias command
      ```
      buichidung@CPU002169:~/Documents/Studying$ unalias ls
      buichidung@CPU002169:~/Documents/Studying$ ls
      awesome-python	Git  gittemp  Project_temp  root  Studying
      buichidung@CPU002169:~/Documents/Studying$
      ```
    Reference Link: https://www.tecmint.com/15-basic-ls-command-examples-in-linux/
 - ## stat
    * Check linux file status
      The easiest way to use **stat** is to provide it a file as an argument. The following command will display the size, blocks, IO blocks, file type, inode value, number of links and much more information about the file /var/log/syslog:
      ```
      buichidung@CPU002169:/$ stat /var/log/syslog
        File: /var/log/syslog
        Size: 337544    	Blocks: 672        IO Block: 4096   regular file
      Device: 802h/2050d	Inode: 56623998    Links: 1
      Access: (0640/-rw-r-----)  Uid: (  104/  syslog)   Gid: (    4/     adm)
      Access: 2021-03-22 06:50:48.397810754 +0700
      Modify: 2021-03-22 14:24:21.420299006 +0700
      Change: 2021-03-22 14:24:21.420299006 +0700
        Birth: -
      ```
    ###Examples:  
    
    * Check file system status
      In the previous example, stat command treated the input file as a normal file, however, to display file system status instead of file status, use the `-f` option.

      ```
      buichidung@CPU002169:/$ stat -f /var/log/syslog
        File: "/var/log/syslog"
          ID: 64f6f9422fad26f Namelen: 255     Type: ext2/ext3
      Block size: 4096       Fundamental block size: 4096
      Blocks: Total: 239965964  Free: 225936043  Available: 213728991
      Inodes: Total: 61022208   Free: 60163401
      ```

      ```
      buichidung@CPU002169:/$ stat -f /
        File: "/"
          ID: 64f6f9422fad26f Namelen: 255     Type: ext2/ext3
      Block size: 4096       Fundamental block size: 4096
      Blocks: Total: 239965964  Free: 225935968  Available: 213728916
      Inodes: Total: 61022208   Free: 60163401
      ```

    * Enable following of symbolic links

      Since Linux supports links  (**symbolic** and **hard links**), certain files may have one or more links, or they could even exist in a filesystem.

      To enable stat to follow links, use the `-L` flag as shown

      ```
      buichidung@CPU002169:/$ stat -L /
        File: /
        Size: 4096      	Blocks: 8          IO Block: 4096   directory
      Device: 802h/2050d	Inode: 2           Links: 21
      Access: (0755/drwxr-xr-x)  Uid: (    0/    root)   Gid: (    0/    root)
      Access: 2021-03-22 06:50:30.781811119 +0700
      Modify: 2020-12-08 10:30:23.302144007 +0700
      Change: 2020-12-08 10:30:23.302144007 +0700
       Birth: -
      ```

    * Use a custom format to display information

      **stat** also allows you to use a particular or custom format instead of the default. The `-c` flag is used to specify the format used, it prints a newline after each use of format sequence.

      Alternatively, you can use the `--printf` option which enables interpreting of backslash escapes sequences and turns off printing of a trailing newline. You need to use `\n` in the format to print a new line, for example.

      ```
      stat --printf='%U\n%G\n%C\n%z\n' /var/log/secure
      ```

      Meaning of the format sequences for files used in above example:

        - **%U** - user name of owner
        - **%G** - group name of owner
        - **%C** - SELinux security context string
        - **%z** - time of last status change, human-readable

      ```
      stat --printf='%n\n%a\n%b\n' /
      ```

      Meaning of the format sequences used in the above command
        - **%n** - shows the file name
        - **%a** - print free blocks available to non-superuser
        - **%b** - outputs total data blocks in file system

    * Print Information in Terse Form

      The `-t` option can be used to print the information in terse form.

      ```
      buichidung@CPU002169:/$ stat -t /var/log/syslog
      /var/log/syslog 338564 672 81a0 104 4 802 56623998 1 0 0 1616370648 1616398501 1616398501 0 4096
      ```

      As a last note, your shell may have its own version of stat, please refer to your shell's documentation for details about the options it supports. To see all accepted output format sequences, refer to the stat man page.

      ```
      man stat
      ```
Reference link: https://www.tecmint.com/linux-stat-command-examples/ 

 - ##grep
    The grep filter searches a file for a particular pattern of characters, and displays all lines that contain that pattern. The pattern that is searched in the file is referred to as the regular expression (grep stands for globally search for regular expression and print out).

    Syntax:
    ```
    grep [options] pattern [files]
    ```

    Options description:

    ```
    Options Description
    -c : This prints only a count of the lines that match a pattern
    -h : Display the matched lines, but do not display the filenames.
    -i : Ignores, case for matching
    -l : Displays list of a filenames only.
    -n : Display the matched lines and their line numbers.
    -v : This prints out all the lines that do not matches the pattern
    -e exp : Specifies expression with this option. Can use multiple times.
    -f file : Takes patterns from file, one per line.
    -E : Treats pattern as an extended regular expression (ERE)
    -w : Match whole word
    -o : Print only the matched parts of a matching line,
     with each such part on a separate output line.

    -A n : Prints searched line and nlines after the result.
    -B n : Prints searched line and n line before the result.
    -C n : Prints searched line and n lines after before the result.
    ```

    Create a file with content following:

    ```
    buichidung@CPU002169:~/Documents/Studying$ nano hello.txt
    ```

    ```
    unix is great os. unix is opensource. unix is free os.
    Unix linux which one you choose.
    uNix is easy to learn.unix is a multiuser os.Learn unix .unix is a powerful.
    ```
    ###Examples:
    * Case insensitive search: The -i option enables to search for a string case insensitively in the give file. It matches the words like "UNIX", "Unix", "unix".

      ```
      buichidung@CPU002169:~/Documents/Studying$ grep -i "UNIx" hello.txt
      unix is great os. unix is opensource. unix is free os.
      Unix linux which one you choose.
      uNix is easy to learn.unix is a multiuser os.Learn unix .unix is a powerful.
      ```

    * Displaying the count of number of matches: We can find the number of liens that matches the given string/pattern

      ```
      buichidung@CPU002169:~/Documents/Studying$ grep -c "unix" hello.txt 
      2
      ```

    * Display the file names that matches the pattern: We can just display the files that contains the given string/pattern
    
      ```
      buichidung@CPU002169:~/Documents/Studying$ grep -l "unix" *
      grep: awesome-python: Is a directory
      grep: Git: Is a directory
      hello.txt
      grep: Project_temp: Is a directory
      grep: root: Is a directory
      grep: Studying: Is a directory
      ```

      or
    
      ```
      buichidung@CPU002169:~/Documents/Studying$ grep -l "unix" hello.txt hello1.txt
      hello.txt
    
      ```
    
    * Checking for the whole words in a file: By default, grep matches the given string/pattern even if it  found as a substring in a file. The -w option to grep makes it match only the whole words.
    
      ```
      buichidung@CPU002169:~/Documents/Studying$ grep -w "unix" hello.txt
      unix is great os. unix is opensource. unix is free os.
      uNix is easy to learn.unix is a multiuser os.Learn unix .unix is a powerful.
      ```  
    
    * Displaying only the matched pattern: By default display the entire line which has the matched string. We can make the grep to display only the matched string by using the -o option.
    
      ```
      buichidung@CPU002169:~/Documents/Studying$ grep -o "unix" hello.txt
      unix
      unix
      unix
      unix
      unix
      unix
      ```
    
    * Show line number while displaying the output using grep -n: To show the line number of file with the line matched
    
      ```
      buichidung@CPU002169:~/Documents/Studying$ grep -n "unix" hello.txt
      1:unix is great os. unix is opensource. unix is free os.
      4:uNix is easy to learn.unix is a multiuser os.Learn unix .unix is a powerful.
      ```
    
    * Inverting the pattern match: You can display the lines that are not matchd with the specified search sting pattern using the -v option.
    
      ```
      buichidung@CPU002169:~/Documents/Studying$ grep -v "unix" hello.txt
      learn operating system.
      Unix linux which one you choose.
      ```
    
    * Matching the lines that start with a string: The ^ regular expression pattern specifies the start of a line. This can be used in grep to match the lines which start with the given string or pattern.
    
      ```
      buichidung@CPU002169:~/Documents/Studying$ grep "^unix" hello.txt
      unix is great os. unix is opensource. unix is free os.
      ```
    
    * Matching the lines that end with a string: The $ regular expression pattern specifies the end of a line. This can be used in grep to match the lines which end with the given string or pattern.
    
      ```
      buichidung@CPU002169:~/Documents/Studying$ grep "oose.$" hello.txt
      Unix linux which one you choose.
      ```
    
    * Specifies expression with -e option. Can use multiple times:
    
      ```
      buichidung@CPU002169:~/Documents/Studying$ $grep –e "Agarwal" –e "Aggarwal" –e "Agrawal" geekfile.txt
      ```
    
    * -f file option Takes patterns from file, one per line.
    
      ```
      buichidung@CPU002169:~/Documents/Studying$ grep -f hello.txt hello1.txt
      ```
    
    * Print n specific lines from a file: -A print the searched line and n lines after the result, -B prints the searched line and n lines before the result, and -C prints the searched line and n lines after and before the result.
    
      Syntax:
    
      ```
      $grep -A[NumberOfLines(n)] [search] [file]  
      $grep -B[NumberOfLines(n)] [search] [file]  
      $grep -C[NumberOfLines(n)] [search] [file] 
      ```
    
      ```
      buichidung@CPU002169:~/Documents/Studying$ grep -A1 learn hello.txt
      learn operating system.
      Unix linux which one you choose.
      uNix is easy to learn.unix is a multiuser os.Learn unix .unix is a powerful.
      ```

    Reference Link: https://www.geeksforgeeks.org/grep-command-in-unixlinux/

 - useradd

 - passwd

 - usermod

</details>

## *Processes Management*
<details>
<summary>Quản lý Processes</summary>
</details>

## *System Resources*
<details>
<summary>Xem tài nguyên hệ thống</summary>

### `top`

This is one of the most frequently used commands in our daily system administrative jobs. This command displays processor activity of Linux box and also displays tasks managed by kernel in real-time.

### Examples:

1. **Display of top command**
        
    ```
    buichidung@CPU002169:~$ top

    top - 09:04:38 up 47 min,  1 user,  load average: 1,02, 1,10, 1,29
    Tasks: 288 total,   1 running, 287 sleeping,   0 stopped,   0 zombie
    %Cpu(s):  5,6 us,  1,4 sy,  0,0 ni, 91,7 id,  1,3 wa,  0,0 hi,  0,0 si,  0,0 st
    MiB Mem :  15897,0 total,   7075,1 free,   4126,0 used,   4695,9 buff/cache
    MiB Swap:   2048,0 total,   2048,0 free,      0,0 used.  10456,6 avail Mem 

    PID USER      PR  NI    VIRT    RES    SHR S  %CPU  %MEM     TIME+ COMMAND                                                             
    4059 buichid+  20   0 4785316 250288 122936 S   6,0   1,5   2:02.43 chrome                                                              
    6988 buichid+  20   0 8943396 180040 100504 S   5,6   1,1   2:49.88 chrome                                                              
    8862 buichid+  20   0 4708804 234228 113488 S   4,0   1,4   0:31.35 chrome                                                              
    3802 buichid+  20   0  897336 204420 120932 S   2,7   1,3   4:14.21 chrome                                                              
    2602 buichid+  20   0 1126968 287424 150416 S   2,3   1,8   2:38.23 chrome                                                              
    2020 buichid+  20   0  870844  77424  45464 S   1,7   0,5   1:49.17 Xorg                                                                
    4665 buichid+  20   0 4882464 798568  65872 S   1,3   4,9   2:07.56 java                                                                
    7386 buichid+  20   0 1976064 252356 112628 S   1,3   1,6   0:53.05 Telegram                                                            
    8921 buichid+  20   0 4658900 176296 126212 S   1,3   1,1   0:07.88 chrome                                                              
    10003 buichid+  20   0  966176  52920  39408 S   1,0   0,3   0:01.61 gnome-terminal-                                                     
    2174 buichid+  20   0 4531392 286724 111584 S   0,7   1,8   2:41.89 gnome-shell                                                         
    670 avahi     20   0    8848   4012   3516 S   0,3   0,0   0:03.30 avahi-daemon                                                        
    1148 mysql     20   0 2199720 361572  34840 S   0,3   2,2   0:06.66 mysqld                                                              
    3810 buichid+  20   0  394324 115224  67216 S   0,3   0,7   0:29.61 chrome                                                              
    6999 buichid+  20   0 9043004 338348 116476 S   0,3   2,1   0:38.68 chrome                                                              
    10053 buichid+  20   0   14868   4312   3528 R   0,3   0,0   0:00.06 top                                                                 
      1 root      20   0  169264  13168   8388 S   0,0   0,1   0:02.26 systemd                                                             
      2 root      20   0       0      0      0 S   0,0   0,0   0:00.00 kthreadd                                                            
      3 root       0 -20       0      0      0 I   0,0   0,0   0:00.00 rcu_gp                                                              
      4 root       0 -20       0      0      0 I   0,0   0,0   0:00.00 rcu_par_gp                                                          
      6 root       0 -20       0      0      0 I   0,0   0,0   0:00.00 kworker/0:0H-kblockd                                                
      9 root       0 -20       0      0      0 I   0,0   0,0   0:00.00 mm_percpu_wq                                                        
     10 root      20   0       0      0      0 S   0,0   0,0   0:00.04 ksoftirqd/0 
    ```
	As we can see, tge default display contains two areas of information: the summary area (dashboard) and the task area (process list). By default, top will be updated after 3 seconds.

	The first line of numbers on the dashboard includes time, how long our computer has been running, the number of people logged in, and what the load average has been for the past one, five, and 15 minutes. The second line shows the number of tasks and their states: running, stopped, sleeping, or zombie.

	The third line displays the following central processing unit (CPU) values:

	- `us`: Amount of time the CPU spends executing processes for people in “user space.”
	- `sy`: Amount of time spent running system “kernel space” processes.
	- `ni`: Amount of time spent executing processes with a manually set nice value.
	- `id`: Amount of CPU idle time.
	- `wa`: Amount of time the CPU spends waiting for I/O to complete.
	- `hi`: Amount of time spent servicing hardware interrupts.
	- `si`: Amount of time spent servicing software interrupts.
	- `st`: Amount of time lost due to running virtual machines (“steal time”).

	The column headings in the process list are as follows:

	- `PID`: The process's ID, a unique positive integer that identifies a process.
	- `USER`: This is the "effective" username (which maps to a user ID) of the user who started the process. Linux assigns a real user ID and an effective user ID to processes; the latter allows a process to act on behalf of another user.
	- `PR and NI`: `NI` field shows the "nice" value of a process. The `PR` field shows scheduling priority of the process from the perspective of the kernel. The nice value affects the priority of a process.
	- `VIRT, RES, SHR and %MEM`: These 3 fields are related with memory consumption of the processes. `VIRT` is the total amount of memory consumed by a process. This includes the program's code, the data stored by the process in memory, as well as any regions of memory that have been swapped to the disk. `RES` is the memory consumed by the process in RAM, and `%MEM` expresses this value as a percentage of the total RAM available. Finally, `SHR` is the amount of memory shared with other processes.
	- `S`: A process maybe in various states. This field shows the process state in the single-letter form.
	- `TIME+`: The total CPU time used by the process since it started, precise to the hundredths of a second.
	- `COMMAND`: Show the name of the processes.

	The status of the process can be one of the following:

	- `D`: Uninteruptible sleep
	- `R`: Running
	- `T`: Traced(stopped)
	- `Z`: Zombie

	Press Q to exit top

2. **Scrolling the Display**

	We can press the Up or Down Arrows, Home, End, and Page Up or Page Down keys to move up and move down and access the processes.

	Press the Left or Right Arrow to move the process list sideways. This is useful to see any columns that don't fit within the confines of the terminal window.


3. **Changing the Numeric Units**

	To change the display units to sensible values. Press capital E to cycle through the units used to display memory values in these options: kibibytes, mebibytes, tebibytes, pebibytes, and exbibytes. The unit in use is the first item on lines four and five.
	
	Press lowercase "e" to do the same thing for the values in the process list: kibibytes, mebibytes, gibibytes, tebibytes, and pebibytes.
	
	We pressed E to set the dashboard memory units to gibibytes and “e” to set the process list memory units to mebibytes.
	
	![image](https://user-images.githubusercontent.com/55236614/112093046-5d9b1a80-8bcb-11eb-8552-3d0ff1c3b89b.png)


4. **Changing the Summary Contents**

	We can change the display settings for the lines in the dashboard or remove them completely.

	Press l to toggle the load summary line (the first line) on or off. We removed the load summary line in the image below.
	
	![image](https://user-images.githubusercontent.com/55236614/112093117-70adea80-8bcb-11eb-8f4d-b9f7f23f1e34.png)

	
	If we have a multi-core CPU, press 1 to change the display and individual statistics for each CPU.
	
	![image](https://user-images.githubusercontent.com/55236614/112093135-7c011600-8bcb-11eb-8efd-04ed7ffbba0c.png)

	
	We can press "t" again to change the graph display to solid block characters or simple ASCII, or press "t" once more to remove the CPU display and task summary line completely.
	
	Press "m" to cycle the memory and swap memory lines through different display options 
	
	![image](https://user-images.githubusercontent.com/55236614/112092926-23ca1400-8bcb-11eb-89f4-cc805c80e2d8.png)

	
5. **Color and Highlighting**

	Press "z" to add color to the display.
	
	![image](https://user-images.githubusercontent.com/55236614/112093272-b4085900-8bcb-11eb-8452-7b51a8a3e854.png)

	Press "y" to highlight running tasks in the process list. Pressing "x" highlights the column used to sort the process list. we can toggle between bold and reversed text highlighting by pressing "b".
	
	![image](https://user-images.githubusercontent.com/55236614/112093429-f3cf4080-8bcb-11eb-8b61-c255ad485a31.png)

	
6. **Sorting by Columns**

	By default, the process list is sorted by the %CPU column. You can change the sort column by pressing the following:

	- P: The %CPU column.
	- M: The %MEM column.
	- N: The PID column.
	- T: The TIME+ column.

	The image below show the process list which is sorted by the %MEM column.
	
	![image](https://user-images.githubusercontent.com/55236614/112093941-e2d2ff00-8bcc-11eb-908d-6a7eb422c5df.png)


7. **See the Full Command Line**

	Pressing "c" toggles the COMMAND column between displaying the process name and the full command line.
	
	![image](https://user-images.githubusercontent.com/55236614/112094087-33e2f300-8bcd-11eb-801b-7c2a66385530.png)

	To see a "tree" of processes that were launched or spawned by other processes, press V

8. **See Processes for a Single User**

	Press "u" to see the processes for a single user. We will be prompted for the name or UID
	
	![image](https://user-images.githubusercontent.com/55236614/112099896-de134880-8bd6-11eb-84f6-3f45d18f74f9.png)
	
	We type the name and hit "Enter" to see the results.

9. **Only see Active Tasks**

	Press "i" to see only active tasks
	
	![image](https://user-images.githubusercontent.com/55236614/112100038-13b83180-8bd7-11eb-8c74-a757965388ce.png)
	
	Task that have not consumed any CPU since the last update will not be shown.

10. **Set How Many Processes to Display**

	Press "n" to limit the display to a certain number of lines. As you can see I typed "10" and hit "Enter". The results will be shown
	
	![image](https://user-images.githubusercontent.com/55236614/112100257-6b569d00-8bd7-11eb-9fb2-345db642c862.png)


11. **Renice the Process**

	We can press "r" to change the nice value (priority) for a process. We will be prompted for the process ID. Press enter to use the process ID of the task at the top of the process list. I typed 2020 as below
	
	![image](https://user-images.githubusercontent.com/55236614/112100544-def8aa00-8bd7-11eb-8cae-6296228f0c92.png)

	Next, we type the new nice value to apply to the prcess. I typed 20, and then press Enter
	
	![image](https://user-images.githubusercontent.com/55236614/112100630-fc2d7880-8bd7-11eb-9889-d5abacb363fe.png)
	
	The new nice value is applied to the process immediately

12. **Kill a Process**

	Press "l" to kill a process. We will be prompted for the process ID we want to kill.
	
	![image](https://user-images.githubusercontent.com/55236614/112101025-97265280-8bd8-11eb-9707-4df1778083bc.png)

	I typed "7386" to kill the process which has COMMAND "Telegram" and press "Enter". Then we will be offered the chance to type the signal we want to send. If we just hit "Enter", top sends the SIGTERM (kill) signal
	
	![image](https://user-images.githubusercontent.com/55236614/112101240-ea000a00-8bd8-11eb-81f4-e7d6492e5a83.png)

	The chosen process was removed immediately.

13. **Customizing the Display**

	We can also customize the colors and columns that are displayed. We’re going to change the color used for prompts, the default for which is red.
	
	Press "Shift + Z" to go to the color settings page. To indicate which display element we want to change, press one of the following, which are case sensitive:
	
	- `S`: Summary Data area.
	- `M`: Messages and prompts.
	- `H`: Column headings
	- `T`: Task information in the process list

	![image](https://user-images.githubusercontent.com/55236614/112102011-30a23400-8bda-11eb-8407-4624f198be3b.png)
	
	We can also change the columns displayed in the Fields Management screen. Press F to enter the Fields Management screen.
	
	![image](https://user-images.githubusercontent.com/55236614/112103079-d2765080-8bdb-11eb-8b0c-d85dffbc4b6e.png)

	While the highlight is on the UID column, we press “s” to sort the process list on the UID column.
	
	Press "s" and "Enter" to save the selection. Then proess "q" to leave the Fields Management screen.
	
	![image](https://user-images.githubusercontent.com/55236614/112103257-12d5ce80-8bdc-11eb-99ee-0c50bd711d82.png)

14. **Alternative Display Mode**

	This works best in full-screen mode. Press A to display four areas in the process list, and then press "a" to move from area to area.
	
	![image](https://user-images.githubusercontent.com/55236614/112103545-7eb83700-8bdc-11eb-901c-6a8bdf6acc6c.png)
	
	Each area has a different collection of columns, but each is also customizable through the Fields Management screen. This gives you scope to have a full-screen, customized display showing different information in each area, and the ability to sort each area by a different column.

15. **Other keystrokes**

	The following are some other keys we might find useful in top:
	
	- `W`: Save you settings and customizations so they will still be in effect when we next start top.
	- `d`: Set a new display refresh rate.
	- `Space`: Force top to refresh its display right now.

### `vmstat`


### `tcpdump`


### `netstat`


### `htop`


### `iotop`


### `iostat`


### `iptraf-ng`


### `iftop`

</details>

## *Installation Packages Management*
<details>
<summary>Quản lý gói cài đặt</summary>
</details>

## *Log Server Management*
<details>
<summary>Quản lý log server</summary>
</details>

## *Console*
<details>
<summary>Trình soạn thảo console</summary>
</details>

## *Network and IPTables*
<details>
<summary>Quản lý Network và IPTables</summary>
</details>

## *Web Server Installation*
<details>
<summary>Cài đặt web server</summary>
</details>

## *Services and Network in Host*
<details>
<summary>Quản lý services/network tại máy chủ</summary>
</details>

## *Other skills*
<details>
<summary>Các kỹ năng khác</summary>
</details>

## *References*
<details>
<summary>References</summary>
</details>
