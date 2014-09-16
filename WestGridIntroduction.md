## Working with the WestGrid Server ##

### Introduction to WestGrid ###
WestGrid is a government funded program designed to provide researchers access to high performance computational machines and distributed data storage. It is a system of nodes (or machines that act like individual computers) whose memory can be used to run complex functions and store data in a way that allows access to many individuals. 

The WestGrid server setup can be visualized like this:

![WestGrid Diagram](https://github.com/mairind/WestGridIntro/blob/master/Images/WestGridDiagram.jpg)

***Figure 1.** A diagrammatic representation of WestGrid's server setup. Your local machine connects to an interactive node, which can pass jobs on to WestGrid via a resource manager controlled by moab. Users can interact with their own local machine, the interactive node, and moab; however, the resource manager and the computing nodes of WestGrid cannot be interacted with directly.*

### Workflow overview ###

*Note: more detail on individual steps further along in this document*

When working with WestGrid, the user interacts with the interactive node. This node serves as a short-term working space and an in-between that separates the local computer from WestGrid's supercomputing nodes. On the interactive node, you can store data, test code, and edit files before sending jobs to the thousands of computing nodes used by WestGrid. The user cannot interact directly with these nodes - they are hidden to the local machine (a "hidden army", as Belaid Moa likes to put it).

After testing your code, if you are ready to send a job to the supercomputers, you create a PBS script. This script includes three vital pieces of information: the memory requirements, the processing requirements, and the wall time that the job will take. These variables are read by a machine called "moab", which decides which jobs to submit to the resource manager (this is to ensure that memory and processing power is distributed optimally). After moab prioritizes the jobs being sent to WestGrid, it informs the resource manager which jobs to pass on to which computing nodes on WestGrid. 


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

It is important here to note the file structure of the interactive node. By logging on to WestGrid, you have opened your home directory on the system. 

### Creating a PBS Script ###
