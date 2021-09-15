# Rebasing
Sources: https://www.atlassian.com/git/tutorials/merging-vs-rebasing#conceptual-overview ; https://git-scm.com/book/en/v2/Git-Branching-Rebasing ; https://www.codeproject.com/Articles/5262688/Advanced-GIT-Tutorial-Interactive-Rebase ; https://mtyurt.net/post/git-using-advanced-rebase-features-for-a-clean-repository.html ; https://itnext.io/advantages-of-git-rebase-af3b5f5448c6

## Introduction
Command '**git rebase**' is a powerful technique of consolidating changes from two branches. The result is only one branch with rewritten **linear history** (follow-up changes not necessarily in chronological order).
Rebasing 'moves' a whole added branch from its roots onto the newest commit of a specified base branch (typically *master/main* or any other branch when collaborating with other developer).
To carry out the action, the HEAD must be set to the added branch and base branch must be specified. The subsequent commands can be used:
```
git checkout <addedbranch>
git rebase <master/main>
```
To fully synchronize the chages, it is always needed to merge (fast-forward) the connected branch to the *master/main*:
```
git checkout <master/main>
git merge <addedbranch>
```

**Advantages**
- In addition to 'git merge' it essentially allows collaboration among coworkers.
+ Linear history is beneficial for your colleagues as they are discovering your content. They do not need to figure out complex merging history. 
+ Rebasing makes the search for mistakes (debugging) more user friendly and it does not create additional unnecessary commits.

## Proper rebase usage - Do's :white_check_mark:

**Interactive rebasing**

- By default, 'git rebase' makes new commits from the original branch one by one. Interactive rebase can edit these commits by opening a text file. It makes the history even cleaner. Simply put **-i** before base branch:
```
git checkout <addedbranch>
git rebase -i <master/main>
```
+ During the editing process, use '**pick**', '**drop**' or '**fixup**' (incorporating one commit to another) to manage the commits. Here is a list of many other orders:
+ 

**Recent commits adjusments**
- 'git rebase' can be specified onto any base (commit), therefore, subsequent code 'moves' a branch onto the current commit:
```
git checkout <addedbranch>
git rebase -i HEAD~2
```
+ This procedure can be definitely helpful since it allows editing (interactive rebasing) the 2 most recent commits on the added branch without actually 'moving'. It is possible to specify commits using hash as well.

**Frequent commiting**
- The number of rebasings in a particular period is without any restrictions. However, it is recommended to rebase constantly to prevent larger conflicts and it keeps the **history clean**. :broom: 

**Multiple branching**
- text
+ text

**list of flags and features**

## Prevention of rebase usage - Don'ts :no_entry:
Public branch
- Avoid rebasing on public branch. Your colleagues could loose their work.
+ text

## Rebasing vs Merging
Commit history: linear against mixed

Re

an extraneous merge commit every time you need to incorporate upstream changes. If *master/main* is very active, this can pollute your feature branch’s history quite a bit. 

![9c4ctjue6vl71](https://user-images.githubusercontent.com/79012119/132845480-9913fca6-3b2a-4771-bfc6-8cd1e96e7c10.jpg)
