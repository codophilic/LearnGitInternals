If we run command `git init Internals` , so this creates an `.git` hidden folder under a folder name as `Internals`. 

Run command `ls -lah` to get the list of files/folder having detailed information. (image 1)

There is no network level operation takes place when we perform any git operations. All operations takes place on local machine under `.git` directory.

So first we created a folder **Internals** which has **0 commits** and has only `.git` directory.

### Getting Visualization of **Internals**`.git` directory (image 2)

The purpose of git is that it provide a version control system to track the snapshots of your project over time.

**Concept of staging area:**

1. When we commit a files before commit the files is placed under an staging area, basically a snapshot of your project at that time. Then you run the command `git add` and `git commit`.





