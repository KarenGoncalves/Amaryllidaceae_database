## Useful commands

Check the [wiki](https://github.com/KarenGoncalves/Amaryllidaceae_database/wiki) to see the list of many useful commands, what they do and how to use them. 

## Commands specific to the server

### Softwares
In the server, you do not need to install most softwares, as they are already installed and are called modules. These modules are what you need to load to your session to be able to use the software.

### Available softwares
- `module avail`
	- Shows the softwares that are installed (available) in the server.
- module spider
	- `module spider softw` shows at all the modules that are installed which contain the words 'softw'. Will show a list of all the versions of those softwares.
	- `module spider software1/1.0` will show a description for software1/1.0, if it is installed,  and how to load it to your session in the server.
- `module keyword softw`
	- Similar to `module spider`, it will search for the word `softw` among the installed modules, but the search extends to the description of the software. This is useful when you are searching for a software that does something specific but you don't need one specific.
- `module load software1`
	- If software1 is installed in the server AND you have loaded all the softwares you need for software1 to work, will load the default version of software1 to your session.
	- If you want an specific version of a software, you need to put it in the command (eg. `module load software1/1.0`)
	- If you have a version of a software loaded and you try to load another, the previous one will be unloaded, meaning you cannot load two versions of a software at the same time.

### `sbatch`
This command will add your script to a queue in the server to run when the required resourcers (time, memory, number of central processing units -- CPUs) are available. You cannot be in your `${HOME}` directory to run it or in folders inside it. You need to move to `${SCRATCH}` or `${PROJECT}` to be able to run.
- `sbatch myScript.sh`
	- Will add myScript.sh to the queue
	- The script must start with the following lines:
		- `#!/bin/bash`
			- Specifies the software (bash) that will be able to read this script (we give it's absolute path, if you type `ls /bin` in the server, you will find the file 'bash')
		- `#SBATCH --time=00-00:00:00`
			- Normally anything that comes after `#` is not read by the computer, but since it is followed by 'SBATCH', the software `sbatch` will read it and interpret that you are asking for this amount of time in the queue.
	- If nothing else is given, the software `sbatch` will allocated the minimum amount of memory and CPUs and will find the sponsor of your account to create the bill the use of the server in the name of the sponsor
		- If you have more than one sponsor (eg. laboidp and desgagne) there will be an error, because the server will not know who they should bill.
		- You can specify the sponsor in two ways:
			- Add the following to a new line in the beginning of your script: `#SBATCH --acount=def-sponsor` (replace "sponsor" by the sponsor of your account)
			- Add the name of the account to the sbatch command: `sbatch --acount=def-sponsor myScript.sh`

### `srun`
Will add your script to the queue, it is as if you were running the script in the session, so until it has finished running you won't be able to do anything else.
- `srun --account=def-sponsor --mem-per-cpu=16G --time=01:00:00 myScript.sh`
	- Will run myScript.sh, billing the account def-sponsor, using a maximum of 16Gb of memory and for maximum 1h.
- `srun --account=def-sponsor --mem-per-cpu=16G --time=01:00:00 -i`
	- Same as above, however there is no script to run because we want to use the resources interactively (`-i`). This is useful if you want to test a script that you know may use a lot of memory. When you are finished, if the time has not run out, you can use `Ctrl + D` or `exit` to go back to your session.

### `salloc`
Similar to using `srun -i`, will allow the interactive use of the resources allocated. When you are finished, if the time has not run out, you can use `Ctrl + D` or `exit` to go back to your session.

### `squeue`
Will show the queue of jobs waiting to run in the server.
- `squeue`
	- Will print ALL the jobs in the queue, submitted by every user of the server, which are running or waiting to run.

- `squeue -u user` or `sq`
	- If your username is `user`, the two commands are the same. Will print all your jobs that are running or waiting to run
	- Below is an example of what the result of `squeue -u smithj` would look like
	
	|JOBID    |USER     |ACCOUNT   |NAME    |ST|TIME_LEFT |NODES|CPUS|GRES  |MIN_MEM|NODELIST (REASON)|
	|:--------|:--------|:---------|:-------|:-|:---------|:----|:---|:-----|:------|:----------------|
	|123456   |smithj   |def-smithj|simple_j|R |0:03      | 1   |1   |(null)|4G     |cdr234     (None)|
	| 123457  |smithj   |def-smithj|bigger_j|PD|2-00:00:00| 1   | 16 |(null)| 16G   |       (Priority)|
	 
	- JOBID: each time you using `sbatch`, `srun` or `salloc`, you create a job and it is assigned an ID, you can use this ID to get information about the job.
	- Name: Unless you specify a different name (by using `--jobname=someName` or `-j someName`) it will be the name of the script submitted
	- ST: status of the job. `R` = running; `PD` = pending, meaning the resources needed are not available so you need to wait; `CG` = completing; `CA` = cancelled; see other status codes <span style="font-weight: bold; text-decoration: underline">[here](https://slurm.schedmd.com/squeue.html#lbAG)</span>
	- Nodes: basically a groupping of CPUs
	- MIN_MEM: memory requested to run the job
		- Nodelist (reason): When your job is running, you will get a list of the nodes it is using. If it is pending, you will get a reason as to why.

### `scancel`
Cancel jobs that are pending or running. Use it with the id of one or more jobs. You are not allowed to cancel a job submitted by someone else.

### `diskusage_report`
Prints a report of the use of the server by you in your home and scratch directories, as well as your directories in the project folders (each sponsor has a different one, so you may have files in different project folders). The report includes the space used and the number of files in each directory, as well as the limits of these directories.
