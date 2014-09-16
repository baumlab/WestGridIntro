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

### <a name="INT"></a> The Interactive Node ### 
There are a number of ways to work with the interactive node - you can access it through the command line (using Terminal or iTerm on Unix systems), or through a number of applications such as FileZilla or any other file transfer protocol (FTP) programs. 

To use the terminal, you access WestGrid's interactive node "Nestor" using any one of the following commands:

`ssh USERNAME@nestor.westgrid.ca`

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
		    /work 		_for scratch work, edited files, and temporary files_
		    
		    /data		_for raw data_
		    
		    /source		_for source code_
		    
		    /software	_for any additional software you might have to install_


#### Running software in the interactive shell ####
Before running any code, a few steps are required.

1. Check to make sure that the software you need (in the correct version) is on the machine by typing:
    cd /global/software
    ls

2. Load any data or scripts required. You can do this using FTP software, or in the command line using `scp FILE_TO_COPY.txt USERNAME@nestor.westgrid.ca /path/to/file/on/nestor`. 

3. Edit your files as needed. For example, change file paths in scripts so that they all point

### Interactive Node File Structure ###

It is important here to note the file structure of the interactive node. By logging on to WestGrid, you have opened your home directory on the system (ie. you are accessing `/home/USERNAME`).  

### Creating a PBS Script <a name="PBSScript"></a> ###

### <a name="moab"></a> Interacting with moab ###