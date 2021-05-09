# Git Notes

### Local repository management

#### Initialize a blank git repository in the current path.

```bash
git init 
```

#### Check the current git repo for modified, added, removed files.

```bash
git status
```

#### Add or remove files to the tracking system.

```bash
git add /src 
git add /src/model.py
git rm /src/model.py
git add -A  // add all the untracked files
```

#### Commit with the message\(-m\) 'add model.py'.

```bash
git commit -m 'add model.py'
```

#### Show the history of the committed versions.

```bash
git log
```



#### Create a _.gitignore_ file to manage which files to and not to track

```bash
touch .gitignore
```

Inside the _.gitignore_ file, the syntax is:

```text
# this is comment
# not to track
*.tif
*.csv
/data

# to track track.csv : using ! to exclude
!track.csv

# the git will stop tracking the csv files except track.csv
```

#### Create a _.gitkeep_  inside one folder to keep blank folder in the repo.

```bash
touch ./.gitkeep
```

#### Go to a certain commit.

```bash
git reset --hard da4b54
```

Here the `da4b54` is the target commit id. It can be checked using `git log` or `git reflog`

### 

### Remote repository management 

 To add a new remote, use the `git remote add` command on the terminal, in the directory your repository is stored at. [https://help.github.com/en/github/using-git/adding-a-rem](https://help.github.com/en/github/using-git/adding-a-remote)[ote](https://help.github.com/en/github/using-git/adding-a-remote)

```bash
git remote add origin git@github.com:aaronzq/segmentation.git
git remote add origin https://github.com/aaronzq/segmentation.git
git remote rm origin
```

'remote' means that this operation is for a remote repo, maybe in github;  
'origin' is just a reference name for that repo, for future abbreviation;  
remote repo on github can be added through ssh or https



SSH setup when using Git Bash on windows

[https://help.github.com/en/github/authenticating-to-github/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent](https://help.github.com/en/github/authenticating-to-github/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent)



Pull and push to the remote repo. 

```bash
git pull origin master
git push -u origin master
```



### The pipeline of cooperation using git and Github

Generally, the cooperation is around one remote repository, for example, on Github. The master branch is the main code of the project, and every team member should pull the code to their local machine and start working on their own branch. When submitting the code, they only submit to their own branch on the Github, NOT the master branch. Next, they create a pull request on Github and let the other team member review it. And finally, their code will be merged into the master branch. 

#### 1.a Create a repository 

On the github.com, create the repository. 

On the local machine, initialize the repository in the source folder:

```bash
git init
git add . 
git commit -m "init the repo"
git remote add origin git@github.com:aaronzq/git_exercise.git
git push -u origin master
```

#### 1.b Clone an existing repository \(e.g. created by your teammate\)

Look for the link on github.com and open git on your local folder

```bash
git clone git@github.com:aaronzq/git_exercise.git
```

#### 2. Create your own branch and start editing the code in your own branch

```bash
git checkout -b feature/fuzeyu/new  // -b means create and go to this branch

// edit some codes

git add .
git commit -m "add codes"
```

Now supposing you are not satisfied with your commit, you can reset to a previous commit in two ways:

```text
git reset 01f1e // the id at the end can be looked up from git log

                // in this reset mode, you move the head to a previous commit, 
                // but the changes
                // you made will remain there

git reset --hard 01f1e
                // no change will remain
```

If you are ready to submit your code to the remote repository on Github, there are also two ways to push:

```text
git push origin feature/zawang // in this mode you can only fast-forward
                               // only able to add non-conflict content
                               
git push -f origin feature/zqwang // force to push, erase the commit history 
                               // on github branch and replace it with your local
                               // ones
```

Some functions that might be useful:

```text
git branch // check how many branches your have and which branch you are at 
git checkout master // go to branch: master
```

#### 3. create pull request on Github 

If there are no conflicts \(where part of your codes is different from existing commits in the master branch\), then merge the pull request and you are good to go.

If there are conflicts, first you should pull the updated master branch to your local. Then, you should solve the conflict locally, using git rebase.

master: A-B-C  
your branch: A-B-D  
After rebase,  
master: A-B-C  
your branch: A-B-C, and waiting for you to solve the conflicts in your editor

```text
git rebase master // you are currently at your own branch, and you are rebase against
                  // the master branch
```

After you manually solve all the conflicts, save the files. 

```text
git add .
git rebase --continue
git push origin feature/zqwang
```

And now your branch will be A-B-C-D. And Github will update your pull request. You are good to fast-forward.

