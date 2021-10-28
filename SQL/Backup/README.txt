// Sqlbackup.bat

sqlcmd -S .\EXPRESS –E -Q "EXEC sp_BackupDatabases @backupLocation='D:\SQLBackups\', @backupType='F'" 

 Example2: Differential backups of all databases in the local named instance of SQLEXPRESS by using a SQLLogin and its password

// Sqlbackup.bat

sqlcmd -U SQLLogin -P password -S .\SQLEXPRESS -Q "EXEC sp_BackupDatabases  @backupLocation ='D:\SQLBackups', @BackupType=’D’"

 Note: The SQLLogin shouldhave at least the Backup Operator role in SQL Server.



Example 3: Log backups of all databases in local named instance of SQLEXPRESS by using Windows Authentication

// Sqlbackup.bat

sqlcmd -S .\SQLEXPRESS -E -Q "EXEC sp_BackupDatabases @backupLocation='D:\SQLBackups\',@backupType='L'"



Example 4: Full backups of the database USERDB in the local named instance of SQLEXPRESS by using Windows Authentication

// Sqlbackup.bat

sqlcmd -S .\SQLEXPRESS -E -Q "EXEC sp_BackupDatabases @backupLocation='D:\SQLBackups\', @databaseName=’USERDB’, @backupType='F'"

Similarly, you can make a differential backup of USERDB by pasting in 'D' for the @backupType parameter and a log backup of USERDB by pasting in 'L' for the @backupType parameter.

Step C: Schedule a job by using Windows Task Scheduler to execute the batch file that you created in step B. To do this, follow these steps:

On the computer that is running SQL Server Express, click Start, point to All Programs, point to Accessories, point to System Tools, and then click Scheduled Tasks. 
Double-click Add Scheduled Task. 
In the Scheduled Task Wizard, click Next. 
Click Browse, click the batch file that you created in step B, and then click Open. 
Type SQLBACKUP for the name of the task, click Daily, and then click Next. 
Specify information for a schedule to run the task. (We recommend that you run this task at least one time every day.) Then, click Next.
In the Enter the user name field, type a user name, and then type a password in the Enter the password field. 

Note This user should at least be assigned the BackupOperator role at SQL Server level if you are using one of the batch files in example 1, 3, or 4.
Click Next, and then click Finish. 
Execute the scheduled task at least one time to make sure that the backup is created successfully.
Note The folder for the SQLCMD executable is generally in the Path variables for the server after SQL Server is installed, but if the Path variable does not list this folder, you can find it under <Install location>\90\Tools\Binn (For example: C:\Program Files\Microsoft SQL Server\90\Tools\Binn).

 Be aware of the following when you use the procedure that is documented in this article:

The Windows Task Scheduler service must be running at the time that the job is scheduled to run. We recommend that you set the startup type for this service as Automatic. This makes sure that the service will be running even on a restart.
There should be lots of space on the drive to which the backups are being written. We recommend that you clean the old files in the backup folder regularly to make sure that you do not run out of disk space. The script does not contain the logic to clean up old files. 
Additional references
Task Scheduler Overview (Windows Server 2008 R2, Windows Vista)
How to Schedule Tasks in Windows XP
Features Supported by the Editions of SQL Server 2008
Properties