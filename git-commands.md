## Git Commands
[Squashing Commits](https://www.devroom.io/2011/07/05/git-squash-your-latests-commits-into-one/)

Reverting to Previous Commit
```bash
git reset --hard <old-commit-id>
git push -f <remote-name> <branch-name>
```
Branches
```bash
# Create a new branch & change into it
git checkout -b <branch-name>

# Switch between branches
git checkout <branch-name>

# Delete a branch
git branch -d <branch-name>
```

Syncing Forks
```bash
# See remotes in current branch
git remote -v

# Add new upstream remote to sync with your fork
git remote add upstream https://github.com/ORIGINAL_OWNER/ORIGINAL_REPOSITORY.git

# Fetch branches and commits from the upstream (will be saved in an "upstream/master" branch)
git fetch upstream

# Merge changes from the upstream into current branch
git merge upstream/master
```
