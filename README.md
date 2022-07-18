# Command line - shell

Absolute path: includes the root (starts with a "/")

Relative path: is dependent on your location in the computer. For example, if the file or folder to which you refer is present in the your current folder, you need simply write its name.

Path here will be used to refer to both absolute and relative paths, unless specified

Session: each time you open a command line window or connect to the server

Script: a list of commands to be executed by the computer

`>` will take what would be printed to the screen and put in a file. What comes before `>` is a command, what comes after it is a file name. If a file of the same name already exists, it will be replaced.

`>>` is similar to `>` but if a file of the same name as we the one we gave already exists, it will simply add what would be printed to the screen to the end of the file instead of replacing it. 

When you start a new line, you start a new command. There are exceptions: 
- `\`: escape character. If you add a new line (press `enter` or `return`) right after a `\`, it will be read as the same line
- Some commands use quotes (either `'` or `"`), and normally if you start a new line without closing the quote, it will be considered part of the same command
- `;` has the same effect as a new line. So you can write multiple commands in the same line by separating them with a `;`

## Commands are normally are acronyms

**Directory** is the same as **Folder**. Working directory means the folder in which you currently are.

`pwd`: *print working directory*: prints the absolute path of your current location in the computer.

`cd`: *change directory*: use `cd` followed by the path to a folder to change your location in the computer.

`ls`: *list*: used alone, it will print the contents of the current directory. If it is followed by a path to a directory, will print its contents. Options (letters or words preceded by "-" or "--", respectively) will change how and what is printed.
- `ls -a` or `ls --all`
	- Will print all contents of the working directory, including hidden files (which start with a dot, eg. .ssh).
- `ls -alhs Desktop` or `ls --all -l --sort --human Desktop`
	- Will print all the contents (`-a` or `--all` including hidden files) in the form of a list (`-l`) that includes the permissions of each file, their owners, the date of the last modification and their size in human format (`-h` or `--human`; 4Kb instead of 4000b) and sort them alphabetically (`-s` or `--sort`; other options can be used to change how to sort the files).
	
`less` or `more`: softwares to open text files as read-only.
- Use `less --help` or `man less` to see a manual on how to use `less` and `more --help` or `man more` to see how to use `more`
- `less file1`
	- Will open file1 to read. If you want to look for a word, type `/` then type the word.
- `less file1 file2 file3`
	- Will open the three files to read. You can move between the files by typing `:` followed by `p` to look at the previous file or `n` to look at the next one.

`cat` : prints file to the screen
- `cat file1`: will print the content of file1 to the screen. If the file is a binary file (.bam), or if it is compressed (.zip, .tar, .gz), the printed text will be all gibberish
- `cat file1 file2`: will print the content of file1 followed by the content of file 2
- `cat file1 > file2; cat file3 >> file2`: is the same as `cat file1 file3 > file2`. 

`head` and `tail`: prints the first (`head`) or last (`tail`) 10 lines of a file to the screen. If you have a big file and you just want to see if it looks fine or what it contains, use this
- `head file1`: will print the first 10 lines of file1
- `head -n 20 file1`: the option `-n` allows you to specify the number of lines you want to be printed

`grep`: searches a text in a file
- `grep "word" file1`: will print to the screen every line of file 1 that contains "word" in it.

`clear`: will clear you screen, so you will not be able to see the previous commands by looking up or scrolling. You can still use the up and down arrows to look at the commands you have used in the session.

`history`: see the list of commands you have used in the computer with the date and time of use. Does not contain ALL the commands you have ever used, since old lines are deleted. So if you think you may need to see the command again, copy it to a file.

`cp`: *copy*: use `cp` followed by the path of the file you want to copy and the path to where you want to put the copy.
- `cp file1 file2`
	- Creates a copy of file1 and names it file2. 
- `cp -r folder1 folder2`
	- If you want to copy a folder, you need to add the option `-r` (recursive, which means it will copy everything inside the folder).
- `cp file1 folder1`
	- Will create a copy of file1 in the folder1 (the copy will be named file1).
- `cp -v -r folder1 folder2`
	- Creates a copy of folder1 and names it folder2. The option `-v` (verbose) will make the computer print to the screen what it is doing (copied folder1/file1 to folder2/file; copied folder1/file2 to folder2/file2; etc). 

`rm`: *remove*: use `rm` followed by the path to a file or folder you wish to delete. It will delete FOREVER, there is no trashbin when you use `rm`, so use with care
- `rm file1 file2 file3`
	- Delete all files listed after the command
- `rm -r folder1`
	- Delete all files in the folder1 and the folder itself (will only work if you use `-r`)
- `rm *`
	- Deletes EVERYTHING (`*`) in the current folder, except folders (since `-r` was not included)
- `rm -v *`
	- Deletes everything in the current folder, except folders, and prints to the screen the files it deleted while doing it.

`rmdir`: remove directory: use with the path to a folder to delete it FOREVER (same as `rm`). Only works if the folder is empty.

`ssh`: *secure shell protocol*: will allow you to connect to a server and write commands into it.
- `ssh user@server.ca`
	- After asking for the password, will log user to server.ca.
- `ssh -i myKey.ppk user@server.ca`
	- The option `-i` means "identity file", ssh will use this file as the password (you need to have it in your local computer and copy it to the server.
	- To create an identity file (also known as ssh-key) follow the instructions <span style="font-weight: bold; text-decoration: underline">[here](https://docs.alliancecan.ca/wiki/Generating_SSH_keys_in_Windows)</span>

`rsync`: will copy files and folders from one location to another. Use it with the path of the files and folders you wish to copy and the path to where you want the copy to be placed. You will be required to put your password with this command.
- `rsync file1 file2 file3 user@server.ca:/home/user/folder1`
	- Will copy the files (file1, file2 and file3) which are in the local computer (eg. the user's laptop) to the directory `/home/user/folder1` which is the remote computer accessed as `server.ca`. The `:` separates the remote computer name from the path to the folder of interest. Here, the path to the folder in the remote computer MUST be the absolute path.
	- If the option `-v` is used, the computer will print what is happening (if the connection to the server was successfull, if it found the folder indicated in the command, what it is copying and if it was successfull)
	- You can use up to 3 v's to get more information on what is happening: `-v`, `-vv` and `-vvv` are all accepted options and sometimes may help you figure out why something is not working.
- `rsync -r folder1 user@server.ca:/home/user/folder1`
	- Just as with `cp` and `rm`, you need to use `-r` to copy a folder with all its contents. This command will result in /home/user/folder1/folder1, because we are copying the whole folder
- `rsync -r folder1/ user@server.ca:/home/user/folder1`
	- This command will copy only the contents of folder1 to the server.
- `rsync -r user@server.ca:/home/user/folder1/ folder1`
	- This command will copy the contents of /home/user/folder1/ from the server.ca to our local computer in folder1

`nano`: software to edit text in the server. Used alone will open an empty file. When you try to close the file (Ctrl + x) will ask if you want to save it and ask for a name for the path where to save it.
- `nano file1`
	- Will open file1 for edition. If file1 does not exist, will create it.
- `nano *`
	- Will open all text files in the working directory for edition in alphabetic order. Once you close one, the next one is opened.

## Commands specific to the server

`module`
- `module avail`
	- Shows the softwares (here they are called modules) that are installed (available) in the server.
- `module spider softw`
	- Shows at all the modules that are installed which contain the words 'softw'. Will show a list of all the versions of those softwares.
- `module spider software1`
	- If software1 is installed, will show its description and all the versions of it that are available.
- `module spider software1/1.0`
	- If the software1/1.0 is installed, will show its description and how to load it to your session in the server.
- `module load software1`
	- If software1 is installed in the server AND you have loaded all the softwares you need for software1 to work, will load the default version of software1 to your session.
	- If you want an specific version of a software, you need to put it in the command (eg. `module load software1/1.0`)
	- If you have a version of a software loaded and you try to load another, the previous one will be unloaded, meaning you cannot load two versions of a software at the same time.

`sbatch`: this command will add your script to a queue in the server to run when the required resourcers (time, memory, number of central processing units -- CPUs) are available. You cannot be in your `home` directory to run it or in folders inside it. You need to move to `scratch` or `project` to be able to run.
- `sbatch myScript.sh`
	- Will add myScript.sh to the queue
	- The script must start with the following lines:
		- `#!/bin/bash`
			- Specifies the software (bash) that will be able to read this script (we give it's absolute path, if you type `ls /bin` in the server, you will find the file 'bash')
		- `#SBATCH --time=00-00:00:00`
			- Normally anything that comes after `#` is not read by the computer, but since it is followed by 'SBATCH`, the software `sbatch` will read it and interpret that you are asking for this amount of time in the queue.
	- If nothing else is given, the software `sbatch` will allocated the minimum amount of memory and CPUs and will find the sponsor of your account to create the bill the use of the server in the name of the sponsor
		- If you have more than one sponsor (eg. laboidp and desgagne) there will be an error, because the server will not know who they should bill.
		- You can specify the sponsor in two ways:
			- Add the following to a new line in the beginning of your script: `#SBATCH --acount=def-sponsor` (replace "sponsor" by the sponsor of your account)
			- Add the name of the account to the sbatch command: `sbatch --acount=def-sponsor myScript.sh`
