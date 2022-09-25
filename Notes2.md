# Deep Dive of Git Internals Object

- As per first notes there are 4 objects in git which helps to track the files , commit etc.

- So here we have a folder **GitInternals2** where we gonna explore more about objects in git. (2)

## Objects in Git

- Since we are viewing objects so we remove the hooks folder using command `rm .git/hooks/*`. (3)

- Creating a file foo.txt which has content 'This is a foo' and add that file in staging area. (4)

- Since its a Blob file it consists of binary data. So to view the actual content run command `git cat-file -p SHA_ID`. (5)

- Now staging area is also called index. So in the .git/index file a binary data which is a temporary database referencing foo.txt.(6)

- The index is a binary file (generally kept in .git/index) containing a sorted list of path names, each with permissions and the SHA1 of a blob object. Run command `git ls-files --staged`.(7)

- The index contains all the information necessary to generate a single (uniquely determined) tree object.
For example, running git commit generates this tree object from the index, stores it in the object database, and uses it as the tree object associated with the new commit. Learn more about it
https://stackoverflow.com/questions/4084921/what-does-the-git-index-contain-exactly (8)

-Now we commit our changes (9). We get two objects (10)

-We add a new file bar.txt and modified foo.txt (11). Adding them in staging area. (12)

- So now we have 5 objects ,3 BlOB,1 tree, 1 commit.(13)

- Now committing the changes. So here, tree object 287633b points two more objects 692fbef5ce6bc1501d972b674d9ad5d0a25f116d and eaad5d8bce9841ca0b9ed2825633e52b989372fb. These two objects are Blobs. (14 & 15)

- Now creating a new file baz.txt and committing it. We get a new commit which points to the tree and that tree is pointing to the all the blobs of each file. (16,17)

## Tracking multiple file having same content.

- So here we create 100 files which has same content using for loop command. (18 19)

- Now adding all files in staging area , git only creates a single object blob for all the files which have content "foo". (20)

- After committing we get more two objects and when we see the tree object it is referencing 100 times to the Blob object. (21 22)

- Created a new branch and changed 100.txt file and its tree object shows the referencing of 100.txt file has been only changed. (23 24) 