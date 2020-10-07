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

Somebody pushed new code into repo. You didn't pulled it, but you made some changes. Then you want to commit and push it:
```bash

```