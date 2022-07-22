
## My Table

| Command                                                                                     | Description                                                                      |
| ------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------- |
| `git checkout --track origin/<name>`                                                        | Checkout remote branch locally                                                   |
| `git checkout [commit ID] [path/to/file]`                                                   | Checkout commit from another branch                                              |
| `git branch -r`                                                                             | List all remote branches                                                         |
| `git branch -a`                                                                             | List all branches                                                                |
| `git branch [name] -d`                                                                      | Delete branch                                                                    |
| `git rm -r --cached .`                                                                      | Remove all files from index in order to reapply gitignore follow it by git add . |
| `git checkout HEAD [filename]`                                                              | Revert a deleted file                                                            |
| `git checkout [branch_name] [paths]`                                                        | Checkout file from another branch                                                |
| `git merge --no-ff --no-commit <branch_name>`                                               | Merge and stage only don't commit                                                |
| `git clone --recurse-submodules <address_repo>`                                             | clone and init submodules                                                        |
| `git cherry-pick <commit>`                                                                  | Apply commit to current branch                                                   |
| `git show some_commit_sha1 -- some_file.c \| git apply -R`                                  | Revert commit for a single file                                                  |
| `git submodule add -b [branch] [repo_url] [./sel_repo_dependencies/[custom_path_you_want]]` | Add submodule                                                                    |
|                                                                                             |                                                                                  |

## My .cfg repo

### checkout another branch 
```
cfg checkout -b work HEAD
```

## Updating Git submodule branch

1.  edit the .gitmodules file to specify the new branch
2.  run git submodule sync
3.  run git submodule update --init --recursive --remote
4.  commit changes
## Git use open ssh

1.  git config --global core.sshcommand "C:/Windows/System32/OpenSSH/ssh.exe"
2.  [https://superuser.com/questions/1296024/windows-ssh-permissions-for-private-key-are-too-open](https://superuser.com/questions/1296024/windows-ssh-permissions-for-private-key-are-too-open)
3.  Print all configs:

git config -l --global
## Resources

* move commits to [another branch](https://howchoo.com/git/git-move-your-latest-commits-to-another-branch#determine-how-many-commits-to-move)