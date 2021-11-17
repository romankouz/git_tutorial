# git_tutorial
A tutorial I have made about git that I made for my reading group. I will try to repurpose this for the general public later if time permits.

# Disclaimer

Not everything listed here is considered best practice. This is just a simple demo and I try to point out where best practice isn't utilized and comment on it, but I want to make it very clear this could definitely not be "best practice". That being said, I hope you learn something :-)

# Getting Started

For this, I have assumed that you have installed git and won't spend time on it here. Installation instructions can be found here: https://www.atlassian.com/git/tutorials/install-git. As of (11/17/2021), anyone reading this should have collaboration access to this repository. I will also assume that you can follow the instructions [here](https://git-scm.com/book/en/v2/Getting-Started-First-Time-Git-Setup) to setup your git credentials. I also highly suggest you follow along in VS Code as that is what I will be using in visual demonstrations.

Then, the initialization is as follows:

1. Open a terminal (any should work).
2. Fork this repository.
3. Type: 
```
git clone https://github.com/(your github username)/git_tutorial
```

# The Basics

 - git add
 - git commit
 - git clone
 - git init
 - git push

Conisder the file 

```
# call this file test_value.py

def test_value():
    assert 1 <= 3
```

We want to add this to our repository. Copy that file into your repository and then run the following:

```
git add .
git commit -m "add test_value file"
git push -u origin main
```

This makes it so that your file you copied locally is now on your global repository!

Note: If you decide to create your own repository with git init, you may need to run

```
git branch -m master main
```

to make sure your local repo is named main and not master. All other steps should be exactly the same.

# Working with Branches


 - git checkout
 - git branch
 - git merge
 - git rebase
 - git diff <branch 1><branch 2>

Let's try and change this text on a different branch: I'm currently on the main branch!

```
git checkout -b markdown_edit # creates a new branch called markdown_edit
git branch -a # shows you all branches including the one you are on (labelled by an asterisk)

```

Now, on this branch, we can change the text! Try it! After you're done, you can push the changes similar to how we did before.

```
git add .
git commit -m "changed markdown file"
git push -u origin markdown_edit
```

Notice that with git diff, you can actually visualize the changes we made on this branch in comparison to the main branch!

```
git diff main markdown_edit
```

# Working with Remote Projects

 - git pull
 - git fetch
 - git push
 - git remote add origin

# Debugging

 - git bisect
 - git grep
 - git blame

The start of this exercise is VERY annoying, but trust me, it's important to see why this is important.

```
git checkout -b bisecting
```

In your ```test_value.py``` file, increase the first value in the assert statement by 1 and perform the following. 

```
git add .
git commit -m "added 1"
git push -u origin bisecting
```

**Repeat this process 5 times**. I promise this is relevant.

```
git bisect start
git bisect bad # <add a bad commit here if you don't want the latest one>
git bisect good <commitSHA> # find an example of a good commit!
```


Repeat this below.
```
python
from test_value import test_value
test_value() # if nothing happens, it means everything is fine.
```

If an AssertionError gets called, then we know that in this version of the project, we had a bad commit. If nothing happens, the test was passing in that version of the project. The outcome determines whether we run ```git bisect bad``` or ```git bisect good```, and we do so respectively to the scenarios I listed.
Repeat this --^

Repeat the process above until git tells you the first bad commit. 

Make sure to reset since bisect doesn't actually operate on any branches, but rather creates its own environment. MAKE SURE TO RESET after you're done so that you return to the current HEAD of your project. If you tack on the HEAD at the end, everything should be fine. You could also just perform git bisect reset after you are done bisecting.

```
git bisect reset HEAD
```

# Cherry Picking

Cherry picking is the fun process of being greedy. We get to quite literally "cherry pick" a commit we like from a different branch, and copy it to the main branch. 

Say I want to make the ```test_value.py``` file to be the version that asserts that 3 (as opposed to 1,2,4, or 5) is less than or equal to 3. Remember that we never merged the bisecting branch with the main branch. This was done intentionally so that we could **cherry pick a desired commit to be added to the main branch and shown here on Github**. You can find the commit history either on the pull request associated with the branch titled bisecting, or you can checkout bisecting in your local repo and get the full commit history with

```
git checkout bisecting
git log --full-history
```

Make sure that you copy the correct commitSHA associated with the commit of interest. 

```
git checkout main
git cherry-pick <commitSHA> # replace the last term entirely with the commitSHA you found
```

This will cause a merge conflict because the desired commit and the one on main have different code for the same line. Resolve this in your editor (GIF of this might be attached later). After resolving, run

```
git cherry-pick --continue
```

This lets git know that you are ready to proceed with the committing process since you have resolved the merge conflict. Finally, push that change to the global main branch!

```
git push -u origin main
```

Tada! You should now see on the main branch that we are asserting that 3 <= 3 and not 1 <= 3.

# "Time Travel"

 - git revert
 - git reset

We won't cover git revert here, since it is the exact inverse of git commit. The only reason you would have to run it is if you want a very convenient way to undo a previous commit. This ease is amplified if there are no merge conflicts.

We will cover git reset here, which allows us to completely change where the HEAD of the project is pointing to and changes the state of the project.

# Starting Over

Using what we learned, we could revert back to the very beginning and start this process all over again. We can accomplish this with git reset! 

**NOTE: Proceeding further will delete any uncommitted changes so proceed with caution.** However, if you want to come back to a commit you made closer to the end of the tutorial, you can run git reflog to find the commit that you want to return to. Don't be scared if the Github commit history showcases it's "gone".

```
git reset --hard veryFirstSHA
git push -f origin main
```

NOTE: The --hard flag for git reset is only useful in this case since you are probably the only one using this dummy repository. In general, it is not adviseable to used the --hard flag. I would consider looking into the --soft and --mixed flag for safer development.

FINAL NOTE: If we want to start all over again with a completely fresh commit history... you may just want to start this whole process (online Github repo creation included) completely from scratch.


