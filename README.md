## GIT workflow
1. Feature branch per task
    - `git checkout -b FEATURE_BRANCH`

2. Push branch regularly
    - `git push origin -u FEATURE_BRANCH`

3. Working
    - `git add -p`
    - `git commit`
    - regularly `git push` (first `git push origin -u FEATURE_BRANCH`)

4. Merge to master
    - `git checkout master`
    - `git merge FEATURE_BRANCH --no-ff -e` -  merge last branch

5. Cleaning
    - `git branch -d FEATURE_BRANCH`
    - `git remote update --prune` - clear


### Tags
- by default they are not send to server
1. Lightweight
    - `git tag NAME COMMIT`
2. Annotated
    - `git tag -a NAME -m "msg" COMMIT`
    - `git push --follow-tags` - send only annotated tags
- `git push origin portfolio` - explicit send tag
- `git fetch --prune --prune-tags` - remove local tags that are not on remote repo


### History - show, log and blame
- `git show @~2` - show second from top
- `git log --online --graph`
- `git log COMMIT..master --oneline --merges --reverse` - show all merge commits from oldest to newest - in reverse - between COMMIT and master
- `git blame PLIK` or `git gui blame index.html`
- `git log -S TEXT -p` - find commit with TEXT in content


### Merge
- do it from destination branch (This branch will stay after merge)
- 3 types: Fast Forward, 3-way merge, octopus merge

### cherry-pick
- move commit from one branch to another
1. `git tag magenta` - tag commit for move
2. `git checkout master` - change branch
3. `git cherry-pick magenta` - move tagged commit

### Diff
- `git diff --word-diff one..two` - show diffs between one and two commits
- `git diff --cached` - shows diffs between working copy and index
- difftool - meld


### Clear working copy
- `git checkout <nothing or commit> -- index.html` - restore file
- `git reset --hard` - remove changes on disk
- clear untracked files
    - `git clean -n` - dry run
    - `git clean -i` - interactive
    - `git clean -f` - force
    - `git clean -fdx` - remove dirs and ignored files


### Commit
- 1 line: summary
- 2 line: empty
- 3 line: details
- max 72 chars in every line
- msg in Past Simple: Added, changed, removed


### Conflicts
- `git reset --hard ORIG_HEAD` - back to state before running dangerous action
- `git config --global merge.tool meld` - set merg tool to meld
- `git config rerere.enabled true` - enable RERERE


### Revert
- `git revert @` - create new commit that restore state


### Rebase
- merge but with linear history
- recommended instead of merge
- do it from feature branch (not master)
- `git rebase -i master` - rebase commits created after moving from master
