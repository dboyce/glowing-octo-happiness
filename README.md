# Pull Request staging check workflow

This repository contains a GitHub custom action called `check-staged` which enforces that a feature branch must be merged into a branch called `staging` 
before it can merged into `master` via a Pull request. 

It works in the following way:

First, it clones the repository with `actions/checkout@v2`

Then it works out what branches have been merged into staging via `git branch -r --no-merge origin/staging`.

It then does the same check with `origin/master` and takes a diff of the branches merged to `origin/staging` that were not found in `origin/master` - 
it stores these branches in a variable called `CURRENTLY_STAGED`. The number of branches in `CURRENTLY_STAGED` should be relatively low compared to the 
total number of branches... (it's effecitvely the amount tickets currently in progress).

For each branch in `CURRENTLY_STAGED` it updates the `staged` status of the head commit to `complete`.

We have a status check for PRs in this repo against the `staged` property...
