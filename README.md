# Git Master Class Activity 2 

This is simple repository to practice some of the concepts taught in Common Code's Git Master Class training course.

Here we look at the effect on commit ID (SHA-1 hashing) of various types of commits.

We look at what the dependencies of the hash operation are and see if we can re-create a certain commit SHA-1.

The only file (apart from this one) is [manuscript.md](https://github.com/commoncode/git-masterclass-activity-1/blob/master/manuscript.md) which is a plain text passage of English.

There is a series of branches already created to set the scene for this activity.

_Try to perform the tasks using commands in your computer terminal - if you get stuck then click 'Show me how'_ 

## Setup

> First off, clone and/or fork this repository locally.

<details><summary>Show me how</summary>
    
    mkdir commoncode
    cd commoncode
    git clone git@github.com:commoncode/git-masterclass-activity-2.git
    cd git-masterclass-activity-2
    
</details>

To restart the exercise at any time just `rm -Rf git-masterclass-activity-2` and re-clone.

-------------------------

This repo has a couple of branches with a sequence of commits resulting in a tree like this:

```
                E --- F --- K --- L     feature_2
               /
        C --- D --- I --- J             feature_1
       /
A --- B --- G --- H                     master

```

In this exercise we will simply look at the tip of the `feature_2` branch.

## Task
>Verify if the same file change on a branch results in the same commit ID.  Do this by _undoing_ and then re-applying commit L.

<details><summary>Show me how</summary>

    git checkout feature_2

    # Copy the last commit info
    git log -n 1 > L.txt

    # See the last change
    git diff HEAD~1

    # Undo the last commit
    git reset --hard HEAD~1

    # Manually redo the commit - s/George/Gary in vim manuscript.md 
    # check the unstaged change then check in
    git diff
    git add manuscript.md 
    git commit -m "L' Gary"

    # Compare the changes
    git log -n 1 > L1.txt
    diff L1.txt L.txt

</details>

## Task
>Verify if a cherry-picked commit results in the same commit ID by reapplying commit L.

<details><summary>Show me how</summary>

    git checkout feature_2

    # Copy the last commit info
    git log -n 1 > L.txt

    # See the last change
    git diff HEAD~1

    # Undo the last commit
    git reset --hard HEAD~1

    git cherry-pick 324c9754d092a8a279714be5bf015ffbcf659c3a

    # Compare the changes
    git log -n 1 > L2.txt
    diff L2.txt L.txt

</details>

## Task
>Generate the a commit with the same SHA-1 has as a previous commit - recreate commit L

To do so we will need to ensure all these are the identical:
    - commit message
    - committer
    - commit date
    - author
    - authoring date
    - parent tree

<details><summary>Show me how</summary>

    git checkout feature_2

    # Copy the last commit info with full details
    git log --pretty=fuller -n 1 > LF.txt

    # Undo the last commit
    git reset --hard HEAD~1

    # Manually redo the commit - s/George/Gary in vim manuscript.md 
    # check the unstaged change then check in
    git diff
    git add manuscript.md 

    # see https://git-scm.com/docs/git-commit-tree
    GIT_COMMITTER_DATE="Fri May 31 17:09:41 2019 +1000" git commit -m "L - Gary" --date "Fri May 31 17:09:41 2019 +1000"

    # Compare the changes
    git log --pretty=fuller -n 1 > LF1.txt
    diff LF1.txt LF.txt

</details>

## Discussion Points
- Do rebased commits behave the same way as cherry-pick commits?
- What is the value of having unique SHA-1 hashes for identical file changes?
- Is there any practical use of overiding timestamps?
