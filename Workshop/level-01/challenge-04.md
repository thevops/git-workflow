# Challenge 03 - Branch integration


## Integration

1. Create merge request (or no if you want merge manually)
2. Review code
3. Merge
    - `git checkout master`
    - `git merge NAME`
    - Fast Forward
    - conflicts
        - resolve conflicts while merging
        - abort merging, rebase master into your branch (resolve conflicts here), back to merge (without conflicts)

## *Clean

1. `git push origin :NAME`
2. `git branch -d NAME`
3. `git remote update --prune`
