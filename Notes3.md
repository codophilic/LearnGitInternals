# Tree pointing Sub-trees

- Create a new git repository name *Internals* 

![](https://github.com/codophilic/LearnGitInternals/blob/main/Notes3/1.PNG)

- Adding some sub folders.

![](https://github.com/codophilic/LearnGitInternals/blob/main/Notes3/2.PNG)

- Created **A1.txt** under *folder/A1* directory and add that it under staging area. We get one BLOB object.

![](https://github.com/codophilic/LearnGitInternals/blob/main/Notes3/3.PNG)

- After committing the files, 4 more git objects are created. 1 is commit , 1 is Blob , 3 are trees (Number of Tree == Number of nested directories + 1 Current directory)

![](https://github.com/codophilic/LearnGitInternals/blob/main/Notes3/4.PNG)
![](https://github.com/codophilic/LearnGitInternals/blob/main/Notes3/5.PNG)

- Similarly created an **A2.txt** file under directory *folder2/A2/* and committed the changes (So total 5 more objects have been added).

![](https://github.com/codophilic/LearnGitInternals/blob/main/Notes3/6.PNG)
![](https://github.com/codophilic/LearnGitInternals/blob/main/Notes3/7.PNG)

- Now we have updated **A1.txt** and committed it. So total we have 15 objects (smart layer compression one object into one folder rather creating new folder object *a8.... object folder*)

![](https://github.com/codophilic/LearnGitInternals/blob/main/Notes3/8.PNG)
![](https://github.com/codophilic/LearnGitInternals/blob/main/Notes3/9.PNG)

- Now we have updated **A2.txt** file under directory *folder2/A2/* and committed the changes ( So total objects 20) 

![](https://github.com/codophilic/LearnGitInternals/blob/main/Notes3/10.PNG)

- Now creating a new file under *folder1/B1* with **B1.txt** with Updating both **A2.txt** and **A1.txt** file in a single commit. 

![](https://github.com/codophilic/LearnGitInternals/blob/main/Notes3/11.PNG)
![](https://github.com/codophilic/LearnGitInternals/blob/main/Notes3/12.PNG)

- Now making **A1.txt** file changed as `A1 A1 ` (previous version) and committing changes. So instead of creating the new blob object of it, git knows there is a same blob of data thats why git directly pointed the tree which consits of previous data. Apart of it, the it also point the latest blob of the other files **B1.txt** and **A2.txt**. This how's git internally works point the content instead of finding the changes.

![](https://github.com/codophilic/LearnGitInternals/blob/main/Notes3/13.jpeg)

- Tracking and pointing is done based on file content and trees are basically  sub-directories

 Meeti meet123456@gmail.com, meet1234



