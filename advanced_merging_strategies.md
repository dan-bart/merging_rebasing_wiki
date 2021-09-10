# Advanced merging startegies

Zdroje:
https://stackoverflow.com/questions/14243397/what-are-gits-merge-strategies
https://stackoverflow.com/questions/366860/when-would-you-use-the-different-git-merge-strategies
https://www.atlassian.com/git/tutorials/using-branches/merge-strategy

## Introduction

The purpose of merging in Git is to apply changes in one branch to another. For example, when new features have been developed and tested in the 'dev' branch, they can be deployed to the production 'main' branch. However, the changes are often not fit for an automatic merge. Some parts of code could have been overwritten, and Git can not determine, which version should it keep. For this reason, merging strategies have been developed.


Resolve
```
git merge -s resolve branch1 branch2
```
Algorithm: Three-way merge
The 'Resolve' strategy relies on finding the first commit that both branches have in common by reading the parent hash ID from the current commit. In the scheme provided, it would be the commit H. The common commit is called 'base' or 'ancestor'.

```
          I--J   <-- main
         /
...--G--H
         \
          K--L   <-- dev
```
Recursive
```
git merge -s recursive branch1 branch2
```
The recursive strategy also uses the three-way algorithm in case of one single ancestor commit. The difference between recursive and resolve is apparent when the ancestor commit is not determinable. Recursive strategy than searches for an ancestor commit of commits H and J, and applies the three-way algorithm to create a inner-merge commit of H and J (hence the name 'Recursive'), wheras 'Resolve' would pick the ancestor at random. By testing on the Linux kernel commits, it has been reported that fewer merge conflicts happen with using the recursive strategy. Recursive is the default strategy for git merge.
```
...--G--H---K--L   <-- main
         \ /
          x
         / \
...--I--J---M--N   <-- dev
```

Octopus

Ours

Subtree

