# Pull requests and rebase #

Git workflow for pull request branch:

1. From master `git checkout -b new-feature`
2. Commit your changes and push to remote branch `git push --set-upstream origin new-feature`
3. Rebase master to be in sync `git fetch && git rebase origin/master`
4. Now because your new-feature branch history is rewritten when you gonna try to push to remote it will be rejected. 
   You can push using force flag `git push -f`. Pull requests is supposed to be temporary branch so this approach should be fine.

