## Working with the WestGrid Server ##

### Introduction to WestGrid ###
WestGrid is a government funded program designed to provide researchers access to high performance computational machines and distributed data storage. It is a system of nodes (or machines that act like individual computers) whose memory can be used to run complex functions and store data in a way that allows access to many individuals. 

The WestGrid server setup can be visualized like this:

![WestGrid Diagram](https://github.com/mairind/WestGridIntro/blob/master/Images/WestGridDiagram.jpg)

When working with WestGrid, the user interacts with the interactive node. There are a number of ways to work with the interactive node - you can access it through the command line (using Terminal or iTerm on Unix systems), or through a number of applications such as FileZilla or any other file transfer protocol (FTP) programs. 

To use the terminal, you access WestGrid's interactive node "Nestor" using any one of the following commands:

    ssh USERNAME@nestor.westgrid.ca

* Login to a server remote using a secure shell (ssh)


    sftp USERNAME@nestor.westgrid.ca

SSH Secure File Transfer Protocol - open the connection using a file transfer protocol


    scp FILE_TO_COPY.txt USERNAME@nestor.westgrid.ca /path/to/file/on/remote/system

Secure copy - copies the file "FILE_TO_COPY.txt" *to the server* from your local machine, pasting it to the remote location you designate


    scp USERNAME@nestor.westgrid.ca:FILE_TO_COPY.txt /local/path/to/file

Secure copy - copies the file "FILE_TO_COPY.txt" *from the server* to the local location you designate