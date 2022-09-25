# Deep Dive of Git Internals Object

- There are 4 objects in git which helps to track the files , commit etc.

- We have a new folder **GitInternals2** where we gonna explore more about objects in git.

![](https://github.com/codophilic/LearnGitInternals/blob/main/Notes2/2.PNG)

## Objects in Git

- Since we are viewing objects so we are ignoring the hooks folder using command `rm .git/hooks/*`.

![](https://github.com/codophilic/LearnGitInternals/blob/main/Notes2/3.PNG)

- Creating a file *foo.txt* which has content *This is a foo* and add that file in staging/index area.

![](https://github.com/codophilic/LearnGitInternals/blob/main/Notes2/4.PNG)

- Since its a Blob file it consists of binary data. So to view the actual content run command `git cat-file -p SHA_ID`.

![](https://github.com/codophilic/LearnGitInternals/blob/main/Notes2/5.PNG)

- Now staging area is also called index. So in the *.git/index* file a binary data which is a temporary database referencing *foo.txt*.

![](https://github.com/codophilic/LearnGitInternals/blob/main/Notes2/6.PNG)

- The index is a binary file (generally kept in .git/index) containing a sorted list of path names, each with permissions and the SHA1 of a blob object. Run command `git ls-files --staged`.

![](https://github.com/codophilic/LearnGitInternals/blob/main/Notes2/7.PNG)

- The index contains all the information necessary to generate a single (uniquely determined) tree object.
For example, running git commit generates this tree object from the index, stores it in the object database, and uses it as the tree object associated with the new commit. [Learn more about it](https://stackoverflow.com/questions/4084921/what-does-the-git-index-contain-exactly)

![](https://github.com/codophilic/LearnGitInternals/blob/main/Notes2/8.PNG)

- Now we commit our changes. We get two objects.

![](https://github.com/codophilic/LearnGitInternals/blob/main/Notes2/9.PNG)
![](https://github.com/codophilic/LearnGitInternals/blob/main/Notes2/10.PNG)

- We add a new file *bar.txt* and modified *foo.txt*. Adding them in staging area. The staging area has two BLOB objects of each file which will be use for new TREE object creation. That new TREE object will be refering these two BLOB objects. 

![](https://github.com/codophilic/LearnGitInternals/blob/main/Notes2/11.PNG)
![](https://github.com/codophilic/LearnGitInternals/blob/main/Notes2/12.PNG)

- So now we have 5 objects ,3 BlOB,1 tree, 1 commit.

![](https://github.com/codophilic/LearnGitInternals/blob/main/Notes2/13.PNG)

- Now committing the changes. So here, tree object `287633b` points two more objects `692fbef5ce6bc1501d972b674d9ad5d0a25f116d` and `eaad5d8bce9841ca0b9ed2825633e52b989372fb`. These two objects are Blobs.

![](https://github.com/codophilic/LearnGitInternals/blob/main/Notes2/14.PNG)
![](https://github.com/codophilic/LearnGitInternals/blob/main/Notes2/15.PNG)

- Now creating a new file *baz.txt* and committing it. We get a new commit which points to the tree and that tree is pointing to the all the blobs of each file.

![](https://github.com/codophilic/LearnGitInternals/blob/main/Notes2/16.PNG)
![](https://github.com/codophilic/LearnGitInternals/blob/main/Notes2/17.PNG)



### Internal Mappings

![](https://github.com/codophilic/LearnGitInternals/blob/main/Notes2/Internals.jpeg)


## Tracking multiple file having same content.

- So here we create 100 files which has same content using for loop command.

![](https://github.com/codophilic/LearnGitInternals/blob/main/Notes2/18.PNG)
![](https://github.com/codophilic/LearnGitInternals/blob/main/Notes2/19.PNG)

- Now adding all files in staging area , git only creates a single object blob for all the files which have content "foo". 

![](https://github.com/codophilic/LearnGitInternals/blob/main/Notes2/20.PNG)

- After committing we get more two objects and when we see the tree object it is referencing 100 times to the Blob object.

![](https://github.com/codophilic/LearnGitInternals/blob/main/Notes2/21.PNG)
![](https://github.com/codophilic/LearnGitInternals/blob/main/Notes2/22.PNG)

- Created a new branch and changed 100.txt file and its tree object shows the referencing of 100.txt file has been only changed. 

![](https://github.com/codophilic/LearnGitInternals/blob/main/Notes2/23.PNG)
![](https://github.com/codophilic/LearnGitInternals/blob/main/Notes2/24.PNG)

## Wondering why TREE objects are pointing BLOBS objects only and not another TREE ?

[See here](https://stackoverflow.com/questions/60247622/does-tree-object-type-in-git-internals-point-to-the-blob-only-or-to-trees-as-wel)
