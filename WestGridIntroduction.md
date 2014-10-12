## Working with the WestGrid Server ##

### Introduction to WestGrid ###
WestGrid is a government funded program designed to provide researchers access to high performance computational machines and distributed data storage. It is a system of nodes (or machines that act like individual computers) whose memory can be used to run complex functions and store data in a way that allows access to many individuals. 

The WestGrid server setup can be visualized like this:

![WestGrid Diagram](https://github.com/mairind/WestGridIntro/blob/master/Images/WestGridDiagram.jpg)

_**Figure 1.** A diagrammatic representation of WestGrid's server setup. Your local machine connects to an interactive node, which can pass jobs on to WestGrid via a resource manager controlled by moab. Users can interact with their own local machine, the interactive node, and moab; however, the resource manager and the computing nodes of WestGrid cannot be interacted with directly._

### Workflow overview ###

*Note: more detail on individual steps further along in this document*

When working with WestGrid, the user interacts with the [interactive node](#INT). This node serves as a short-term working space and an in-between that separates the local computer from WestGrid's supercomputing nodes. On the interactive node, you can store data, test code, and edit files before sending jobs to the thousands of computing nodes used by WestGrid. The user cannot interact directly with these nodes - they are hidden to the local machine (a "transparent army", as Belaid Moa likes to put it).

After testing your code, if you are ready to send a job to the supercomputers, you create a [PBS script](#PBSScript). This script includes three vital pieces of information: the memory requirements, the processing requirements, and the wall time that the job will take. These variables are read by a machine called ["moab"](#moab), which decides which jobs to submit to the resource manager (this is to ensure that memory and processing power is distributed optimally). After moab prioritizes the jobs being sent to WestGrid, it informs the resource manager which jobs to pass on to which computing nodes on WestGrid. 

### <a name="INT"></a> The Interactive Node 
There are a number of ways to work with the interactive node - you can access it through the command line (using Terminal or iTerm on Unix systems), or through a number of applications such as FileZilla or any other file transfer protocol (FTP) programs. 

To use the terminal, you access WestGrid's interactive node "Nestor" using any one of the following commands:

`ssh -XY USERNAME@nestor.westgrid.ca`

* Login to a server remote using a secure shell (ssh)


`sftp USERNAME@nestor.westgrid.ca`

* SSH Secure File Transfer Protocol - open the connection using a file transfer protocol


`scp FILE_TO_COPY.txt USERNAME@nestor.westgrid.ca /path/to/file/on/remote/system`

* Secure copy - copies the file "FILE_TO_COPY.txt" **to the server** from your local machine, pasting it to the remote location you designate


`scp USERNAME@nestor.westgrid.ca:FILE_TO_COPY.txt /local/path/to/file`

* Secure copy - copies the file "FILE_TO_COPY.txt" **from the server** to the local location you designate

There are many more commands available to you, but these are a good starting place for most purposes. By using these commands (or an FTP application) you can upload code and data to WestGrid. From there, you can edit and compile code, write scripts, change file structures, test code, and essentially perform any functions that you would be able to do in your own terminal. Because WestGrid's nodes already have a good deal of software installed, there is plenty that can be done in the interactive node.

#### Helpful commands to use while working with the interactive node: ####
`hostname` shows which machine you are accessing through Nestor

`pwd` = print working directory, shows which directory you are currently in

`ll -a` lists all files, including hidden ones

<!---
To generate a more user-friendly command line interface, use these scripts (from Belaid Moa):
    scp bmoa@localhost:~/.bash_profile .
    scp bmoa@localhost:~/.usrbashrc .bashrc
-->

You will also want to make a global scratch folder, which is another term for a folder that you can work in that will not be backed up on the server.

`mkdir /global/scratch/USERNAME`

While you are at it, creating a file structure is a very, very good idea. Not only willl you be improving your own ability to navigate on the interactive node, but this will be very helpful to others coming in to help you (it is not a good idea to upset those you rely upon for tech support)!

A good file structure could be as follows:

    /home
	    /USERNAME
		    /work 		*for scratch work, edited files, and temporary files
		    
		    /data		*for raw data
		    
		    /source		*for source code
		    
		    /software	*for any additional software you might have to install

Any folder you might want to make can be created by running `mkdir DIRECTORYNAME`. Note: this will create a folder within the folder you are currently working in. For example, if you are working in your `/data` folder and run `mkdir source`, you will now have a source folder nested within your data folder.

#### Running software in the interactive shell ####

Before running any code, a few steps are required.

1.**Check software lists** to make sure that the software you need (in the correct version) is on the machine by typing:

    cd /global/software
    ls

If the software you need is not included, you'll need to either contact Belaid Moa (bmoa@uvic.ca) or install the software yourself in your /software folder. 

2.**Load the software you are going to need**. The administrators of WestGrid have made this very easy! Once you have the name of the software you need (R, for example), type

    module avail R        		*Lists the available versions of R
    module load MODULE_REQUIRED	*Loads the software you require
    which R        				* Returns the version of R currently being used on the machine

3.**Load any data or scripts required.** You can do this using FTP software, or in the command line using 

    scp FILE_TO_COPY.txt USERNAME@nestor.westgrid.ca /path/to/file/on/nestor

Alternatively, if you data are on GitHub, you can load it using:
    
    git clone https://github.com/.../Project_Name

4.**Edit your files if needed.** For example, change file paths in scripts so that they all point to the right places. There are a number of ways to edit files on Nestor, but a good place to start is using vim:

    vi NAME_OF_FILE

Alternatively, for a graphical text editor:

    module load jedit		*Load text editing software
    jedit NAME_OF_FILE &

This will open an external text editor called "jedit".

#### Running R Code ####

After editing your code, you can test it by turning your R code into a script. To run your code as a script, insert a line above your R code that reads:

    #!/bin/env RScript

After saving your code, you can then run it using:

    Rscript CODE_NAME.R

### Creating a PBS Script <a name="PBSScript"></a> ###

After your code is tested and your job is ready to be passed to the computational nodes of WestGrid (see [best practices](#bestprac) for advice on how to arrange your jobs efficiently), you need to create a PBS script for each job you want to send to the cluster of nodes.

If you want to run many small jobs, a PBS script must be created for each job you want to submit. This can be automated to save time (for more information, contact Belaid Moa - he is willing to guide new users through this process). 

There are two fundamental pieces to a PBS script: an initial requirements block followed by the script itself. It is easiest to illustrate the process of creating a PBS script with an example. 

Assume that I want to create a job entitled "Env_Covar_Job". It will run an R script called "Env_Covars.R". This script will require 2 hours to run with 2GB of memory, and I only think that 1 core will be required.

To open the script, I write `vi Env_Covar.pbs`. This will open a script in the terminal window. In this script, I layout my two sections (requirements and script):
    
    #!/bin/bash -l 
    # PBS -N Env_Covar_Job			*Name of the job
	# PBS -l walltime=2:00:00		*I expect it to take 2 hours
	# PBS -l procs=1 				*It will require 1 core - if you exceed this value, your job will be killed
	# PBS -l mem=2gb 				*It needs 2GB of memory
	# PBS -j oe 					*If there is an error, take the standard output and error output to put into the single file; otherwise the nodes will create both an output file and an error file
	# PBS -M EMAIL@uvic.ca 			*Where to send alert emails
	# PBS -m bea 					*When to send an email; can be when the job B(egins), E(nds), and/or A(borts)

Above is the first section that lists the requirements. It is read by moab, and this information dictates when and where your job will be sent to in WestGrid.

Below the requirements, you outline your code:

	cd $PBS_0_WORKDIR 				*The directory from which the script is run is loaded automatically as your working directory
	module load R/R_VERSION 		*Load the R software required for the code
	echo "Starting program..." 		*This is not required - it simply sends a message to the terminal letting you know that it has begun
	Rscript Env_Covars.R 			*Run the RScript
	echo "End program with exit status $? at 'date'"


The entire PBS script looks like this:

	#!/bin/bash -l 
    # PBS -N Env_Covar_Job			
	# PBS -l walltime=2:00:00		
	# PBS -l procs=1 					
	# PBS -l mem=2gb 					
	# PBS -j oe
	# PBS -M EMAIL@uvic.ca 				
	# PBS -m bea

	cd $PBS_0_WORKDIR
	module load R/R_VERSION
	echo "Starting program..." 
	Rscript Env_Covars.R 		
	echo "End program with exit status $? at 'date

Note that the requirements section is written with comment characters at the beginning of each line - these comments indicate which lines should be read by moab and which should be ignored. The first uncommented line indicates where moab will stop reading, so the structure of your PBS script is very important! 

After writing a PBS script, it is submitted to moab using

	qsub Env_Covar.pbs

At this point, moab reads the requirements for your script and sends it to the resource manager, which then sends it to the appropriate computational node of WestGrid depending on what your script requires. Your job is given a ticket number which may come in handy in case you run into technical troubles. You can check the ticket numbers of jobs you have running by typing `qstat -u USERNAME`. 

### <a name="moab"></a> Interacting with moab ###

There are a number of commands that can be used to interact with moab. 

	qdel JOB_NAME		*Deletes a job
	qstat JOB_NAME		*Status of the queue
	qsub JOB_NAME		*Submit a job
	checkjob JOB_NAME	*Check the status of a job
	canceljob JOB_NAME	*Cancel a job (more forceful than qdel)

### After the Code is Complete ###

After the WestGrid servers have completed running your code, you will want to move the results back to your local machine using 

	scp USERNAME@nestor.westgrid.ca:FILE_TO_COPY.txt /local/path/to/file


### <a name="bestprac"></a> Best Practices of Using WestGrid for Computation, including Parallelization ###

When submitting a large job to WestGrid, try to split your job into multiple smaller jobs. This way, multiple nodes can be used to complete your task, not only reducing the total time it will take to run the code, but also making it easier for your code to begin running. In addition, by creating sub-jobs that work simultaneously, it is far easier to cancel your script if an unexpected error comes up - preventing a waste of not only your time, but also server time.

To break up R scripts into multiple jobs, consider parallelization. There are a number of R packages that allow for parallelization, including [parallel](https://stat.ethz.ch/R-manual/R-devel/library/parallel/doc/parallel.pdf), [mpi](http://cran.r-project.org/web/packages/Rmpi/index.html), [snow](http://cran.r-project.org/web/packages/snow/snow.pdf), and [snowfall](http://cran.r-project.org/web/packages/snowfall/index.html). These packages allow for different nodes to work in tandem, relaying information back and forth as required. Parallelization spreads computation and data amongst different nodes, decreasing the memory required for each internal process and increasing the speed of computation overall. 
