- When we run command `git init Internals` it creates an *.git* hidden folder under a parent folder name *Internals*. 

- There is no network level operation takes place when we perform any git operations. All operations takes place on local machine under *.git* directory.

- Get the list of files/folders complete details run command `ls -lah`

![](https://github.com/codophilic/LearnGitInternals/blob/main/Git-Internal-Images/1.JPG)

- So here, first we created a folder **Internals** which has **0 commits** and has only *.git* directory.

- The purpose of git is that it provide a version control system to **track the snapshots of your project over time**.

## 1. Objects in Git

- There are 4 objects which git stores and every object has a hash which is the identifier:
**1. BLOB (Binary Large Objects)**: A *BLOB* is used to store file data- it is generally a file.
**2. TREE**:A *TREE* is basically like a directory- it references a bunch of other trees and blobs (i.e. files and sub-directories).
**3. COMMIT**:A *COMMIT* object holds metadata for each change introduced in the repository, including the author, committer, commit-data, and log- messages.
**4. TAGS**: A *TAG* object assigns an arbitrary human-readable name to a specific object usually a commit.



## 2. Getting Visualization of Internals directory

1. Before committing all the files, first it is placed under an staging area (`git add`) and after that we commit (`git commit`) it so basically a snapshot of your project at that time take place internally. 

2. So on a linux system (ubuntu) under **Internal** folder we have created an empty `Readme.md` file and we are listing out *.git* files.

![](https://github.com/codophilic/LearnGitInternals/blob/main/Git-Internal-Images/Images/3.PNG)

3. Before adding it in staging area we run a command `watch -n .5 tree .git` to view all changes take place under *.git* directory for every 0.5 seconds. (It will update the tree in every half seconds).

![](https://github.com/codophilic/LearnGitInternals/blob/main/Git-Internal-Images/Images/4.PNG)

4. So after running the `git add` command , git does not move the file, it copies the file contents in the staging area creating a **BLOB object** of it.

![](https://github.com/codophilic/LearnGitInternals/blob/main/Git-Internal-Images/Images/5.PNG)

5. So when we run `git add` command , git takes the file, see the contents of the file and took those contents and made a copy in a object folder which are called as BLOB. BLOB's don't have names its a collection of data. So it took those files data , added some more information like what type of file it is? lenght of the files etc. and ran a SHA algorithm which outputs a 40 character hex. ( e69de29b... ). So git took the raw data (BLOB), took some information and ran a SHA.

6. Now git need to take a snapshot of a current state directory or a checkpoint in time which references the directory at this point time, so we run command `git commit`. These creates two objects which are **TREE** and **COMMIT**.

![](https://github.com/codophilic/LearnGitInternals/blob/main/Git-Internal-Images/Images/6.PNG)

7. To find out which is the tree object and which one is a commit object , run command `git show commit_ID`. This command is called **plumbing command**. Its gives an overview about the SHA.

- **`git show e69de29` states the Object is a blob with empty data i.e our Readme.md file.**

![](https://github.com/codophilic/LearnGitInternals/blob/main/Git-Internal-Images/Images/7.PNG)

- **`git show c0dec67` states its a commit object having information like author name , commit message etc.**

- **`git show b8d76bc` states its a TREE object.**

![](https://github.com/codophilic/LearnGitInternals/blob/main/Git-Internal-Images/Images/8.PNG)

8. Get the detailed information about the object by running command `git show --pretty=raw commit_ID`.

![](https://github.com/codophilic/LearnGitInternals/blob/main/Git-Internal-Images/Images/8.PNG)

9. After running the command `git commit`, git creates a snapshot of that time with given commit message **```create empty readme```** which is represented by the TREE object. The TREE object represents your working directory at that point of time.

![](https://github.com/codophilic/LearnGitInternals/blob/main/Git-Internal-Images/Images/9.PNG)

10. The tree object consists of few differents parts

- **100644 -> 100 means its a regular file (1-100 it ranges).**

- **644 is the UNIX permission. So even if changed permission git recognizes it as a modified file.** 

So inside the tree there are list of items which are permission of a file, referenced of a BLOB hash and file name.

![](https://github.com/codophilic/LearnGitInternals/blob/main/Git-Internal-Images/Images/10.PNG)

11. ![#f03c15]**File name is not present in BLOB and it is present in TREE?**

- Suppose there are multiple files having same contents, git will reference the same BLOB if there are multiple tree for those files! or you rename your file? git will create a new tree for you instead creating a new BLOB.

12. So tree SHA is calculated based on BLOB and commit SHA is calculated based on the tree.

                                    Commit
                                    /   \
                                Tree     Tree..
                                /  \
                            BLOB    SubTree.



## 3. Refs/Head branches

1. Branch master is pointing to the latest commit object.
**On a branch** states that when a new commit is made on the branch then the current branch will point to the latest commit object.

![](https://github.com/codophilic/LearnGitInternals/blob/main/Git-Internal-Images/Images/11.PNG)

2. So instead of doing `git show c0dec67` , `git show master` will also output the same.



## 4. Multiple files committed 

1. Now a new file is created and added in staging area and then committed.

![](https://github.com/codophilic/LearnGitInternals/blob/main/Git-Internal-Images/Images/12.PNG)
![](https://github.com/codophilic/LearnGitInternals/blob/main/Git-Internal-Images/Images/13.PNG)

2. After running commit command, there is a new entry stating **Parent** .**Objects in git are immutable** , so going to the past object making them change such that the past object points the future object is impossible (old_commit->new_commit). That's why the future commit points to the parent commit (old_commit<-new_commit).

![](https://github.com/codophilic/LearnGitInternals/blob/main/Git-Internal-Images/Images/14.PNG)

3. Git does not stores differences when a file is modified , its creates a new object having the changes. So it takes more spaces? 
- In git there is an **compression layer** which takes all those blob in which same files has been modified multiple times and packs them in a file.



## 5. Checkout

1. So i added here a new file,committed those changes and again modified it committed it and perform checkout of the first commit. 

![](https://github.com/codophilic/LearnGitInternals/blob/main/Git-Internal-Images/Images/15.PNG)
![](https://github.com/codophilic/LearnGitInternals/blob/main/Git-Internal-Images/Images/16.PNG)
![](https://github.com/codophilic/LearnGitInternals/blob/main/Git-Internal-Images/Images/17.PNG)

2. All the other files disappears. **So internally git goes to the history and finds the commit object ,through that commit object finds refereces which are trees or substrees referencing to blobs and makes a identical copy of all of those references on the directory within O(1) time because its uses Merkel tree or Hash tree as a data structure. Git ignore its own folder.**

![](https://github.com/codophilic/LearnGitInternals/blob/main/Git-Internal-Images/Images/18.PNG)
![](https://github.com/codophilic/LearnGitInternals/blob/main/Git-Internal-Images/Images/19.PNG)

2. Since we have created a new referance from a commit ID , we need to provide that refs a name.

![](https://github.com/codophilic/LearnGitInternals/blob/main/Git-Internal-Images/Images/20.PNG)

## 6. Rebase and rewrite history

- Suppose accidently some wrong commits are made and now the history consists of it. So we can rewrite the history avoiding the wrong commits. Let's say these were my wrong commits
**`7867517 Adde a new file`**

- So to delete this commit use `git rebase -i HEAD~3`.

![](https://github.com/codophilic/LearnGitInternals/blob/main/Git-Internal-Images/Images/21.PNG)
![](https://github.com/codophilic/LearnGitInternals/blob/main/Git-Internal-Images/Images/22.PNG)

## 7. REFLOG

- Suppose accidently we rebase commit which was not supposed to be so to get it back even a version control system and a back sort of version control.

- So `git reflog` , reflog keeps all the history of references when if its rebase or rewrite.

- So run `git reset --hard HEAD{number}` to get those changes.

![](https://github.com/codophilic/LearnGitInternals/blob/main/Git-Internal-Images/Images/23.PNG)
![](https://github.com/codophilic/LearnGitInternals/blob/main/Git-Internal-Images/Images/24.PNG)


![Learn more about git internals](https://medium.com/mindorks/what-is-git-object-model-6009c271ca66#:~:text=Git%20Object%20Store%201%20A%20%E2%80%9Cblob%E2%80%9D%20is%20used,author%2C%20committer%2C%20commit-data%2C%20and%20log-%20messages.%20More%20items)







