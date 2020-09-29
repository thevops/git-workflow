# Challenge 03 - Workflow steps



## Setup

1. Add SSH key to your profile
2. `git clone REPO`
    - over SSH


## Checkout
Updates files in the working tree to match the version in passed commit, index, tree.

1. `git checkout master`
2. `git checkout -b NAME`

<details><summary>Why?</summary>
<p>

- parallel working (versions, workers)
- experiments
- history log
- independent working

</p>
</details>


<details><summary>Branch naming</summary>
<p>

- feat-some-keywords
- bug-some-keywords
- task-some-keywords

</p>
</details>

<details><summary>Some tips</summary>
<p>

- **head** - pointer to last commit (tip) on every branch (.git/refs/heads)
- **HEAD** - current commit (alias @) (.git/HEAD)

</p>
</details>


## Working

1. `git add -p`
2. `git commit` (`-m`)
3. `git push -u origin NAME`

<details><summary>Commit message</summary>
<p>

- 1 line: summary
- 2 line: empty
- 3 line: details
- max 72 chars in every line
- message in Past Simple: Added, changed, removed

</p>
</details>

