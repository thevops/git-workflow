Use Cases with solutions
========================

Return from 'detached HEAD'
---------------------------

### Problem
```bash
✔ 13:50 /tmp/git-workshop [master|✔] $ git checkout d545a0f2a49e82878ccda56ba05a92d279374d0b
Note: checking out 'd545a0f2a49e82878ccda56ba05a92d279374d0b'.

You are in 'detached HEAD' state. You can look around, make experimental
changes and commit them, and you can discard any commits you make in this
state without impacting any branches by performing another checkout.

If you want to create a new branch to retain commits you create, you may
do so (now or later) by using -b with the checkout command again. Example:

  git checkout -b <new-branch-name>

HEAD is now at d545a0f Merge branch 'somthing' into 'master'
✔ 13:50 /tmp/git-workshop [:d545a0f|✔] $ git status
HEAD detached at d545a0f
nothing to commit, working tree clean
```

### Solution
`git checkout master` or `git checkout -` (back to last branch)


Save changes that you created before pulling new version
--------------------------------------------------------

### Problem
Somebody pushed new code into repo. You didn't pulled it, but you made some changes. Then you want to commit and push it:
```bash
✔ 13:57 /tmp/git-workflow [master|✔] $ vim README.md # add your code
✔ 13:57 /tmp/git-workflow [master|✚ 1] $ git add .
✔ 13:57 /tmp/git-workflow [master|●1] $ git commit -m "new changes"
[master 54944dd] new changes
 1 file changed, 1 insertion(+)
✔ 13:58 /tmp/git-workflow [master ↑·1|✔] $ git push
To github.com:vizarch/git-workflow.git
 ! [rejected]        master -> master (fetch first)
error: failed to push some refs to 'git@REPO_URL/git-workflow.git'
podpowiedź: Updates were rejected because the remote contains work that you do
podpowiedź: not have locally. This is usually caused by another repository pushing
podpowiedź: to the same ref. You may want to first integrate the remote changes
podpowiedź: (e.g., 'git pull ...') before pushing again.
podpowiedź: See the 'Note about fast-forwards' in 'git push --help' for details.

```

### Solution

- move you code into different (new) branch
- back to master, reset and pull new code
- rebase your branch

```bash
✔ 14:00 /tmp/git-workflow [master ↑·1|✔] $ git checkout -b new-code
Switched to a new branch 'new-code'
✔ 14:00 /tmp/git-workflow [new-code L|✔] $ git checkout -
Switched to branch 'master'
Your branch is ahead of 'origin/master' by 1 commit.
  (use "git push" to publish your local commits)
✔ 14:01 /tmp/git-workflow [master ↑·1|✔] $ git log # look at
✔ 14:01 /tmp/git-workflow [master ↑·1|✔] $ git reset --hard origin/master
HEAD is now at 3e87567 new
✔ 14:01 /tmp/git-workflow [master|✔] $ git checkout new-code 
Switched to branch 'new-code'
✔ 14:03 /tmp/git-workflow [new-code L|✔] $ git rebase -i master # !!! integrate master changes in your code
Successfully rebased and updated refs/heads/new-code.
✔ 14:06 /tmp/git-workflow [new-code L|✔] $ git checkout master # back to master
Switched to branch 'master'
Your branch is up to date with 'origin/master'.
✔ 14:06 /tmp/git-workflow [master|✔] $ git merge new-code # merge your code
Updating 4242f78..cfcaccc
Fast-forward
 README.md | 1 +
 1 file changed, 1 insertion(+)

# now you can push
```
