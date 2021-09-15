# Rebasing
Sources: https://www.atlassian.com/git/tutorials/merging-vs-rebasing#conceptual-overview ; https://git-scm.com/book/en/v2/Git-Branching-Rebasing ; https://www.codeproject.com/Articles/5262688/Advanced-GIT-Tutorial-Interactive-Rebase ; https://mtyurt.net/post/git-using-advanced-rebase-features-for-a-clean-repository.html 

## Introduction
Command "git rebase" is a powerful technique of consolidating changes from two branches. The result is only one branch with linear rewritten history (follow-up changes not necessarily in chronological order).
Rebasing moves a whole added branch from its roots onto the newest commit of a specified base branch (typically master/main).
To carry out the action, the HEAD must be set to the added branch and base branch must be specified. The subsequent commands can be used:
```
git checkout <addedbranch>
git rebase <master/main>
```


## Proper rebase usage -Do's
Advantages
- text
+ text

Interactive rebase
- list of flags and features
+ text

Multiple branching
- text
+ text

Recent commits adjusments
- text
+ text

Frequent commiting
- text
+ text

## Prevention of rebase usage -Don'ts
Public branch
- Avoid rebasing on public branch. Your colleagues could loose their work.
+ text

## Rebasing vs Merging
Commit history: linear against mixed

![9c4ctjue6vl71](https://user-images.githubusercontent.com/79012119/132845480-9913fca6-3b2a-4771-bfc6-8cd1e96e7c10.jpg)
