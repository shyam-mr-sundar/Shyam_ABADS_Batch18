# Shyam_ABADS_Batch18
<PRE>Power BI Assignment Git don't allow more than 100 MB to attach and 
Git Bash commit is also failing with same error. 
Git Gui and Git Bash both are failing.

**error message :** 
```
$ git push origin main
Uploading LFS objects: 100% (1/1), 230 MB | 7.9 MB/s, done.
Enumerating objects: 19, done.
Counting objects: 100% (19/19), done.
Delta compression using up to 2 threads
Compressing objects: 100% (17/17), done.
Writing objects: 100% (17/17), 219.96 MiB | 4.34 MiB/s, done.
Total 17 (delta 7), reused 0 (delta 0), pack-reused 0 (from 0)
remote: Resolving deltas: 100% (7/7), completed with 1 local object.
remote: error: Trace: add650d9241c48f550b4b6091c0a16344aa9f2630fbed03f1cad61c9d91161ab
remote: error: See https://gh.io/lfs for more information.
remote: error: File Shyam_ABADS_Batch18.zip is 219.31 MB; this exceeds GitHub's file size limit of 100.00 MB
remote: error: File Power BI/Shyam_ABADS_Batch18.zip is 219.08 MB; this exceeds GitHub's file size limit of 100.00 MB
remote: error: GH001: Large files detected. You may want to try Git Large File Storage - https://git-lfs.github.com.
To https://github.com/shyam-mr-sundar/Laptop_to_Cloud_git.git
 ! [remote rejected] main -> main (pre-receive hook declined)
error: failed to push some refs to 'https://github.com/shyam-mr-sundar/Laptop_to_Cloud_git.git'
  ```

1️⃣_Primary method:_

 Import Power BI template and then add two fields earlierst data = 12-08-2008 and latest date 01-03-2021.
Once Template is loaded then change the source file location.
* File ➡️ Options and Settings ➡️ Data Source Settings.
* There are 4 connections each connection need to be linked to respective file.
* Load the data and then wait for visualization to refresh.


 
 2️⃣ Alternate method if we don't have source files .
 
So I'm following a method which is known to me for uploading multiple zip files.

<img width="1001" height="731" alt="image" src="https://github.com/user-attachments/assets/14c464f2-571a-4310-b00c-f960d00fa775" />

Step 1: Install 7zip a free software to manage zip files. <href>https://www.7-zip.org/</href>
Step 2: Create a folder and extract all the contents from file *.zip.001 to *.zip.022
Step 3: Select File *.zip.001 and right click. Then select Combine files.
Step 4: Look for single Zip file with all the contents intact.</PRE>
