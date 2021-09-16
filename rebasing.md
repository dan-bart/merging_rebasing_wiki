# Rebasing

## Introduction
Command '**git rebase**' is a powerful technique of consolidating changes from two branches. The result is only one branch with rewritten **linear history** (follow-up changes not necessarily in chronological order).
Rebasing 'moves' a whole added branch from its roots onto the newest commit of a specified base branch (typically *master/main* or any other branch when collaborating with other developers).
To carry out the action, the HEAD must be set to the added branch and base branch must be specified. The subsequent commands can be used:
```
git checkout <addedbranch>
git rebase <master/main>
```
To fully synchronize the changes, it is always needed to merge (fast-forward) the connected branch to the *master/main*:
```
git checkout <master/main>
git merge <addedbranch>
```

**Advantages**
+ Linear history is beneficial for your colleagues as they are discovering your content. They do not need to figure out complex merging history. 
+ Rebasing makes the search for mistakes (debugging) more user friendly and it does not create additional unnecessary commits.

## Proper rebase usage - Do's :white_check_mark:

**Interactive rebasing**

- By default, 'git rebase' solves conflicts and makes new commits from the original branch one by one. Interactive rebase can edit these commits by opening a text file. It makes the history even cleaner. Simply put **-i** before base branch:
```
git checkout <addedbranch>
git rebase -i <master/main>
```
+ During the editing process, use '**pick**', '**drop**' or '**fixup**' (incorporating one commit to another) to manage the commits. Here is a list of many other orders:

```
p, pick = use commit
r, reword = use commit, but edit the commit message
e, edit = use commit, but stop for amending
s, squash = use commit, but meld into previous commit
f, fixup = like "squash", but discard this commit's log message
x, exec = run command (the rest of the line) use shell
b, break = stop here
d, drop <commit> = remove commit
l, label <label> = label current HEAD with a name
t, reset <label> = reset HEAD to a label
m, merge = creates a merge commit using the original commit message
```
  
**Recent commits adjusments**
- 'git rebase' can be specified onto any base (commit), therefore, subsequent code 'moves' a branch onto the current commit:
```
git checkout <addedbranch>
git rebase -i HEAD~2
```
+ This procedure can be definitely helpful since it allows editing (interactive rebasing) the 2 most recent commits on the added branch without actually 'moving'. It is possible to specify commits using hash as well.

**Frequent commiting**
- The number of rebasings in a particular period is without any restrictions. However, it is recommended to rebase constantly to prevent larger conflicts and to keep the **history clean**. :broom: 

**Multiple branching**
- Naturally, it happens that there are additional branches created from yet existing branches.
```
           K -- L            <-- master/main
         /
...--G--H
         \
           I -- J -- O       <-- added1
            \
              M -- N         <-- added2
              
```
+ Imagine that *added2* branch is complete and should be incorporated into *master/main*. 'git rebase' offers an interesting --onto option:
```
git rebase --onto <master/main> <added1> <added2>
```
+ The result is the following:
```
           K -- L            <-- master/main
         /       \
...--G--H          M' -- N'  <-- added2
         \
           I -- J -- O       <-- added1
              
```
+ Now it is crucial to execute a fast-forward merge as described in the Introduction:
```
           K -- L            
         /       \
...--G--H          M' -- N'  <-- added2 <-- master/main
         \
           I -- J -- O       <-- added1
              
```
+ Further imagine that *added1* branch is complete and should be incorporated into *master/main*. We proceed with standard rebasing and executing fast-forward merge:
```
git rebase <master/main> <added1>
```
```
           K -- L            
         /       \
...--G--H          M' -- N' -- I' -- J' -- O'  <-- added2 <-- added1 <-- master/main     
```
To conclude the work, the branches can be deleted since they are incorporated:
```
git branch -d added1
git branch -d added2
```

**list of flags and features**

## Prevention of rebase usage - Don'ts :no_entry:
Public branch
- Avoid rebasing on public branch. Your colleagues could loose their work.
- Avoid rebasing pushed commits. If you already pushed commits to remote and other people have used them as base for their work, rebasing would create unnecessary merge conflicts.
- If it happened, one possibility is to 'git rebase' again since Git may distinguish the changes if the commits are similar. However, do this only with extreme confidence.

## Rebasing vs Merging
Keeping a linear history of commits is not always preferred. For example, after developing a self-consistent feature, there will probably be no need to go back and observe the individual commits, so merging would be a better tool, as we would end up with the feature changes bundled up as one big commit during the merge. Moreover, the feature commits are then nicely bundled when looking at the repository history. Conversely, if a developer made many unrelated changes that do not have a common framework outline, it is best to use rebase.


an extraneous merge commit every time you need to incorporate upstream changes. If *master/main* is very active, this can pollute your feature branch’s history quite a bit. 


![9c4ctjue6vl71](https://user-images.githubusercontent.com/79012119/132845480-9913fca6-3b2a-4771-bfc6-8cd1e96e7c10.jpg)

### Sources:  
https://www.atlassian.com/git/tutorials/merging-vs-rebasing#conceptual-overview  
https://git-scm.com/book/en/v2/Git-Branching-Rebasing   
https://www.codeproject.com/Articles/5262688/Advanced-GIT-Tutorial-Interactive-Rebase  
https://mtyurt.net/post/git-using-advanced-rebase-features-for-a-clean-repository.html  
https://itnext.io/advantages-of-git-rebase-af3b5f5448c6  
