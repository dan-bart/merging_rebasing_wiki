# Advanced merging startegies

Zdroje:  
https://stackoverflow.com/questions/366860/when-would-you-use-the-different-git-merge-strategies  
https://www.atlassian.com/git/tutorials/using-branches/merge-strategy  
https://git-scm.com/docs/git-merge  
https://stackoverflow.com/questions/9069061/what-is-the-difference-between-git-merge-and-git-merge-no-ff  
https://marc.info/?l=linux-kernel&m=139033182525831  
https://yunwuxin1.gitbooks.io/git/content/en/166dfa9a3724f8ec184652066005eef6/b106e7f9582f0a0eb1d787c4b8a1d093.html  

## Introduction

The purpose of merging in Git is to apply changes from one branch to another. For example, when new features have been developed and tested in the 'dev' branch, they can be deployed to the production 'main' / 'master' branch. However, the changes are often not fit for a fast-forward merge. Some parts of code could have been overwritten, and Git could not be able to determine, which version it should keep. For this reason, merging strategies have been developed.

### Strategies

### (no) Fast-forward
```
git merge (--no-ff)
```
This type of merge is the most simple one. It is used when the changes are linear with respect to the HEAD of branch. In other words, there has not been any commit to the branch other than the one we have just prepared for a merge. The default option, fast-forward merge, will simply move the HEAD pointer to the latest commit. If we want to clearly seperate the last few commits as a new branch, we include the `--no-ff` flag.

The following code shows comparison when opting not to use Fast-forward. Using `--no-ff` allows someone reviewing history to clearly see the branch you checked out to work on.

#### With fast-forward
```
...--G--H
         \    
          I--J   <-- main
```


#### Without fast-forward
```
...--G--H------- K <-- main
         \    /
          I--J   <-- feature
```

### Resolve
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
### Recursive
```
git merge -s recursive branch1 branch2
```
The recursive strategy also uses the three-way algorithm in case of one single ancestor commit. The difference between recursive and resolve arises only when the ancestor commit is not determinable, as is the case in the graph below. Recursive strategy then searches for an ancestor commit of commits H and J, and applies the three-way algorithm to create a inner-merge commit of H and J (hence the name 'Recursive'), wheras 'Resolve' would pick the ancestor at random. By testing on the Linux kernel commits, it has been reported that fewer merge conflicts happen with using the recursive strategy. Recursive is the default strategy for git merge.
```
...--G--H---K--L   <-- main
         \ /
          x
         / \
...--I--J---M--N   <-- dev
```

### Octopus
```
git merge -s octopus branch1 branch2 branch3 branchN
```

The octopus merge is useful when several branches have been developed simultaneously for a longer period of time. The benefit of this method is that is creates only a single commit, which looks cleaner than having one commit per each pair of branches. On the other hand, solving merge conflicts for multiple branches can be too lengthy. Therefore, it is recommended to use the octopus merge with a maximum of 15 branches, before it turns into a Cthulhu merge.

![image](https://user-images.githubusercontent.com/79012119/133561603-9e6365ed-cba4-408f-9407-086eada642a3.png)



### Ours
```
git merge -s ours branch1 branch2 branchN
``` 
This strategy combines the histories of two branches, but does not apply any commits made in branch2. As a result, we move the common ancestor, which enables an easier merging for later branches. This can be useful when we want to get rid of some old feature branches which are long past the current development state of branch1. There is no "theirs" version, since we can just checkout to the other branch and perform the "ours" merge from it.


### Subtree
```
git pull/merge -s subtree main-branch sub-branch
```
The Subtree strategy is useful when we are dealing with a sub-repository nested in another repository. The sub-repository would typically be located in a subdirectory of the main project.

First we have to put the repo in the subdirectory, while keeping the histories separate.
```
git read-tree --prefix=*dir-name*/ -u sub-branch
```
After committing some changes in the sub-branch, we can then checkout to the main-branch, and use the Subtree strategy to update main-branch with files from the sub-branch.


Ostensibly Recursive's Twin

New 2021 strategy, TODO  
https://www.theregister.com/2021/08/17/git_233/  
https://lore.kernel.org/git/xmqq1r6touqi.fsf@gitster.g/  




