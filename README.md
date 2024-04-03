# Linux-Commands-and-Shell-Scripting-Project
This is a Shell Scripting project of the Hands-on Introduction to Linux Commands and Shell Scripting course on Coursera. This project also a part of IBM Data Engineering Professional Certificate. This script essentially creates a backup of files modified within the last 24 hours in the specified target directory and saves it in the specified destination directory using a compressed tar archive format. You will find all of task commands on the backup.sh file.

Scenario

You are a lead linux developer at the top-tech company “ABC International INC.” ABC currently suffers from a huge bottleneck - each day, interns must painstakingly access
encrypted password files on core servers, and backup those that were updated within the last 24-hours. This introduces human error, lowers security, and takes an unreasonable
amount of work.As ABC INC’s most trusted linux developer, you have been tasked with creating a script backup.sh which automatically backs up any of these files that have been updated within the past 24 hours.

Objectives

The objective of this lab is to incorporate much of the shell scripting you’ve learned over this course into a single script.
You will schedule your shell script to run every 24 hours using crontab.
TIP: If you’re unsure whether some of your code will work as wanted, you can try the command directly in the terminal - and even create your own test scripts!

Task 1
Navigate to # [TASK 1] in the code.
Set two variables equal to the values of the first and second command line arguments, as follows:
1. Set targetDirectory to the first command line argument
2. Set destinationDirectory to the second command line argument

# [TASK 1]
targetDirectory=$1

destinationDirectory=$2


Task 2: Display the values of the two command line arguments in the terminal.

# [TASK 2]

echo "targetDirectory is $1"

echo "destinationDirectory is $2"

Task 3:  Define a variable called currentTS as the current timestamp, expressed in seconds.
Click here for Hint

# [TASK 3]
currentTS=$(date +%s)

/*We're going to:
  
  1: Go into the target directory
  
  2: Create the backup file
  
  3: Move the backup file to the destination directory
  

To make things easier, we will define some useful variables...
*/

Task 4: Define a variable called backupFileName to store the name of the archived and compressed backup file that the script will create.
The variable backupFileName should have the value "backup-[$currentTS].tar.gz"

# [TASK 4]
backupFileName="backup-$currentTS.tar.gz"

For example, if currentTS has the value 1634571345, then backupFileName should have the value backup-1634571345.tar.gz. 
Task 5: Define a variable called origAbsPath with the absolute path of the current directory as the variable’s value.

# [TASK 5]
origAbsPath=$(pwd)

Task 6: Define a variable called destAbsPath with value equal to the absolute path of the destination directory.

# [TASK 6]
cd $destinationDirectory

destAbsPath=$destinationDirectory

Task 7: Change directories from the current working directory to the target directory targetDirectory.

# [TASK 7]
cd $origAbsPath

cd $targetDirectory

Task 8
You need to find files that have been updated within the past 24 hours.
This means you need to find all files who’s last modified date was 24 hours ago or less.

# [TASK 8]

yesterdayTS=$(($currentTS - 24 * 60 * 60))

/*
Task 9:  Within the $() expression inside the for loop, write a command that will return all files and directories in the current folder.
 
Task 10: Inside the for loop, you want to check whether the $file was modified within the last 24 hours.

Task11:  In the if-then statement, add the $file that was updated in the past 24-hours to the toBackup array
*/


for file in $(ls) do  // [TASK 9]
  if ((`date -r $file +%s` > $yesterdayTS))  // [TASK 10]
  then
     toBackup+=($file) // [TASK 11]
  fi
done


Task 12: After the for loop, compress and archive the files, using the $toBackup array of filenames, to a file with the name backupFileName.
# [TASK 12]
tar -czvf $backupFileName ${toBackup[@]}


Task 13: Now the file $backupFileName is created in the current working directory. Move the file backupFileName to the destination directory located at destAbsPath.

# [TASK 13]
mv $backupFileName $destAbsPath

Task 14 : If you would like to verify if the my script is excutable, please download the backup.sh to your local computer and make sure save the file as backup.sh in order to make it work

Task 15:  Verify the file is executable using the ls command with the -l option:
<img src="https://github.com/Ellapanstumpe/Linux-Commands-and-Shell-Scripting---Final-Project/blob/main/15-executable.jpg" width="100">


Task 16
1. Download the following zip file with the wget command:
   
# [TASK 16-1]
wget https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBM-LX0117EN-SkillsNetwork/labs/Final%20Project/important-documents.zip

2. Unzip the archive file:
# [TASK 16-2]
unzip -DDo important-documents.zip

3.Update the file’s last modified date to now:
# [TASK 16-3]
touch important-documents/*

4. Test your script using the following command:
# [TASK 16-4]
 ./backup.sh important-documents .
 
This should have created a file called backup-[CURRENT_TIMESTAMP].tar.gz in your current directory
Task 17: Take a screenshot of the output of ls -l and save as 16-backup-complete.jpg (or .png) 


Test the cronjob to see if the backup script is getting triggered by scheduling it for every 1 minute.
Please note that since the Theia Lab is a virtual environment, we need to explicitly start the cron service using the below command.

sudo service cron start

Once the cron service is started, please check in the directory (/home/project) if the tar files are getting created.
If yes, then please stop the cron service using the below command, else it will continue to create tar files every minute.

 sudo service cron stop
 
Using crontab, schedule your /usr/local/bin/backup.sh script to backup the important-documents folder every 24 hours to the directory (/home/project).

Take a screenshot of the output of crontab -l and save as 17-crontab.jpg (or .png)
Ensure the cron service is running or start the cron service if need be, when you are setting up cron jobs in a real-life scenario.
Task 18:  Save the current working file (backup.sh) with CTRL-S [Windows/Linux], ⌘-S [MAC] or navigating to File->Save as seen below:



