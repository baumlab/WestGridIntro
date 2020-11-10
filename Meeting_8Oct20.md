# Kristina meeting with Belaid 8 Oct 2020

Hermes is being decommisioned  
Dont put anything else on there

Compute canada is a national/federal organziation that gets money   
* CC is not for profit   
* different proviences  
* West grid   
 * BC/ UVIC  
* Acenet 
* QC 
* Sharnet 
* Scinet


CPU/cores
memory
GPU


1. Cedar cluster -> this is the SFU super computer - 100,000 cores (24-48 cores per node), 1400 GPUs, 125 gb - 3tb memory
2. Beluga and graham as well they are similar


Node
- cores
- GPU
- memory
- storage


ssh to connect the head node
on macs you need xquartz to show the windows from the server
need a ssh client (for windows is mobaxterm, linnux you dont need anything, mac is just terminal)


log into: `ssh -XY email@cedar.computecanada.ca`   
to edit file: `nedit file.txt`  
exit: `exit`   
to see nodes and their information (dont really need to do): `sinfp`  
to see files: `ll` or `ls`
file type code: drwxrwxr-x
d (at the begining) = directory 
- (at the begining) = regular file
l (at the begining) = shortcut
- the rest of the letters are the permissions.  
	- owner (the first three)
	- group (the next three)
	- others (the next three)
Permissions:  
read (r) - see what is in it
write (w) - edit it
excutable	(x) - run it

so drwx--S--- baum  def-baum   so this is a folder that Julia as the owner as read, write and excutable permissions, the group does not have have read or write permissions

small s - you can cross the folders   

learn more about file: `file file.txt` or `file applications`
 
 For Baum lab
 def-baum is the group
 
 `diskusage_report` get information as different folders, space used and how much I can use, number of files
 
 `ll -a` shows everything including hidden files. 
 
 in diskusage.  
 - home  
 - scratch - not backed up and so this is where you work and then you save final in the project    
 - project (group ktietjen)  - dont use because no storage there.  
 - project (group def-baum) - only use this project, verly slow to backup so this is why you work in the scratch folder and then just put the final version here  

 
 `chgrp baum filename`   to change the ownership of a file/folder
 
 scp will let you copy files from your machine to the serve. 
 cyberduck will also let you copy files. 
 or filezilla. 
 winscp also. 
 compute canada has a nice web wiki on how to transfer files.  
 
 globus - tranfer tiles between clusters, and from computer to the server (would have to install globus personal) - instructions on the computer canada website - compute cananda pays for globus that transfers between clusters
 
 `nearline` - to archive files, you can still access it but it will be slow to read
 
 `scratch` gets purged every 2 months so if you havent touched a file in scratch for 2 months then it will get deleted - so move it to project. 
 dont use `scp` to copy it over use `rsync -rtl ~/scratch/......`
 information on the compute canada website.  
 
# Run code

cvmfs = software   
`module ava` = shows all the software available  
`module spider r` show me everything that has r in it  
`module spider r/4.0.2` to load it  
`module load nixpkgs/16.09 gcc/7.43.0`   
`which r` what r is loaded  
`module unload r/4.0.2`  to unload it  
`R` gives information on r  
`install.packages("dplyr")` to instal packages - you will have to say yes to putting it in your personal library    



`mkdir name` make a directory
`R`
`source(filename.R)` to run the r code


or add `#!/usr/bin/env/Rscript` at the top of the r script but you have to make sure that you have the x permision if not:
`chmod +x file name` to give the x permission

# send a job to the node
`sq` show only my running jobs   
`squeue` show 

Write a job script (PBS script or slurm script or batch script).  
part 1 - what resources are needed to run script
part 2 - the code you want to run

`nano submit_r` - open job script (or `nedit`)
Start with `#!/bin/bash`
 #SBATCH --job-name=yourjobname
 #SBATCH --nodes=1
 #SBATCH --tasks-per-node=1
 #SBATCH --cpus-per-task=1
 #SBATCH --mem=4000 (you can just put the maximum of that node - 128000)
 #SBATCH --time=day-hr:min:sec
 
then add in software loaded `module load gcc/7.3.0 r/4.0.2`
then run script `./exple.R	`

 
submit the job `sbatch jobscriptname.sl`

look at output `


look at job times `sacct`
so over estimate at first and then you can look at the above and adjust the resources needed in the job script if you need to resubmit 


# interactive version of using the nodes
`salloc --nodes=1 --ntasks=1 --cpusper-task=1 --mem=4000M`
now you are the node and have up to 3 hrs of interactivity
you can launch rstudio - google rstudio compute canada

 
 