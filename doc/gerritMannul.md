## How to delete remote branch git

### 1.First look Your remote branch cmd
``git branch -r``

### 2.Then delete remote branch as below (**Be careful**)
```
git branch -r -d origin/remote_branch_name
git push origin :remote_branch_name
```
