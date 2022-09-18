If we run command `git init Internals` , so this creates an `.git` hidden folder under a folder name as `Internals`. 

Run command `ls -lah` to get the list of files/folder having detailed information. (image 1)

There is no network level operation takes place when we perform any git operations. All operations takes place on local machine under `.git` directory.

So first we created a folder **Internals** which has **0 commits** and has only `.git` directory.

### Getting Visualization of **Internals**`.git` directory (image 2)

The purpose of git is that it provide a version control system to track the snapshots of your project over time.

**Concept of staging area:**

1. When we commit a files before commit the files is placed under an staging area, basically a snapshot of your project at that time. Then you run the command `git add` and `git commit`.

2. So on a linux system (ubuntu) under **Internal** folder we have an empty `Readme.md` file. (Image3)

3. Before adding it in staging area we run a command `watch -n .5 tree .git` to view all changes take place under `.git` directory. (It will update the tree in every half seconds) (Image4)

4. So after running the `git add` command , git does not move the file, it copies the file in the staging area creating an Object of it (image5)

#### What are Objects in Git?

There are 4 objects which git stores and every object has a hash which is the identifier:
1. BLOB: Data of files

2. TREE
3. COMMIT
4. TAGS

5. So when we run `git add` command , git took the file see the contents of the file and took those contents and made a copy in a object folder which are called as BLOB (Binary Large Objects). BLOB's don't have names its a collection of data. So it took that data , added some more information like what type of file it is? lenght of the files etc. and ran a SHA algorithm which outputs a 40 character hex. ( e69de29b... ). So git took the raw data (BLOB) , took some information, ran a SHA.

6. Now we need to take a snapshot of a current state directory or a checkpoint in time which references the directory at a point time, so we run command `git commit`. These creates two objects which are TREE and COMMIT. (6)

7. To find out which is the tree object and which one is a commit object , run command `git show commit_ID`. This command is called plumbing command. Its gives an overview about the SHA.
`git show e69de29` states the Object is a blob with empty data i.e our Readme.md file.
`git show c0dec67` states its a commit object having information like author name , commit message etc.
`git show b8d76bc` states its a TREE object.

8. Get the detailed about the object by running command `git show --pretty=raw commit_ID`. (9)

9. After running the command `git commit` we create a snapshot of that time with given commit message (create empty readme) which is represented by the TREE object. The TREE object represents your working directory at that point of time.

10. (10)The tree object consists of few differents parts, 100644 -> 100 means its a regular file (1-100 it ranges), 644 is the UNIX permission. So even if changed permission git recognizes it as a modified file. So inside the tree there are list of items which are permission of a file, referenced of a BLOB hash and file name.

11.File name is not present in BLOB and it is present in TREE? suppose there are multiple files having same contents, git will reference the same BLOB if there are multiple tree for those files! or you rename your file? git will create a new tree for you instead creating a new BLOB.

12. Commit-->TREE-->BLOB. So tree SHA is calculated based on BLOB and commit SHA is calculated based on the tree.
## Refs/Head branches

1. So the branch master is pointing to the latest commit object.
**On a branch** states that if a new commit will be made then the current branch will point to the latest commit object. (11)

2. So instead of doing `git show c0dec67` , `git show master` will also output the same.

## Multiple files committed 

1. So a new file is created and added in staging area and then committed. (12&13)

2. After running command (14) there is a new entry stating **Parent** . Objects in git are immutable , so going to the past object making them change such that the past object points the future object is impossible (old_commit->new_commit). That's why the future commit points to the parent commit (old_commit<-new_commit).

                                    Commit
                                    /   \
                                Tree     Tree..
                                /  \
                            BLOB    SubTree.


3. Git does not stores differences when a file is modified , its creates a new object having the changes. So it takes more spaces? in git there is an compression layer which takes all those blob in which same files has been modified multiple times and packs them in a file.

## checkout

1. (15,16)So i added here a new file,committed those changes and again modified it committed it (17) and perform checkout of the first commit. All the other files disappears (18 & 19). So behind the scenario is goes to the history and finds the commit object then through that commit object checks which trees or substrees it referencing or blobs and makes a identical copy of it on the directory. ( git is smart enough to ignore its own folder )

2. Since we have created a new referance from a commit ID , we need to provide that refs a name. (20)

## Rebase and rewrite history

1. Suppose accidently some wrong commits are made and now the history consists of it. So we can rewrite the history avoiding the wrong commits. Let's say these were my wrong commits
7867517 Adde a new file

So to delete this commit use `git rebase -i HEAD~3`. (21&22)

## REFLOG
(23 24)
Suppose accidently we rebase commit which was not supposed to be so to get it back even a version control system and a back sort of version control.
So `git reflog` , reflog keeps all the history of references when if its rebase or rewrite.
So run `git reset --hard HEAD{number}` to get those changes.










