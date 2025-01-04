---
tags:
  - Wizardry
date: 2024-04-27T21:43
---
<!-- 2024-04-27-2143 (April 27, 2024 09:43:47 PM) -->

# Git Wizardry

`git` is a based tool for version control management or source control management

## General Usage
- best for quick commits and push (if just want to save, best for solo)
```bash
# stage ALL changed or added files
git add -A
# commit the staged files (save local copy)
git commit -m "commit-message"
# save to remote copy on branch main
git push origin main
```

- see changes and unstaged files
```bash
git status
```

- see commit messages
```bash
git log
```

## Squashing Commits via Interactive Rebase

### 1. Start Interactive Rebase:
- the `n` is the number of latest commits you want to squash
```bash
git rebase -i HEAD~n

# example for squashing 4 previous commits:
git rebase -i HEAD~4
```

### 2. Selecting What to Squash and Pick
- then you will see something like:
`<pick|squash> <commit-hash> <commit-message>`
```bash
pick b54366fd777 new: something added docs
squash efjshife7738 fix: commit fix
squash f79gvr7934 docs: new fix
```
- edit the buffer/file by selecting what to squash with `squash` and what to `pick`: 
  - the top commit should be `pick`, you may reorganize the order of the commits
  - there should only be 1 `pick` and the rest should be marked as `squash`
- after editing, **SAVE AND CLOSE** the file.

### 3. Resolve Conflicts 

- to resolve conflicts, open the interactive rebase:
```bash
git rebase --edit-todo
# or just open the file with the conflicts
```

- add a final commit-message at the top of the buffer/file
- then run:
```bash
git rebase --continue
```
- then you're done for the local commit.

### 4. Pushing Changes to Remote
- normal push
```bash
git push origin <branch-name>
```
- OR force push with lease -> force push but first checks if local branch is up-to-date with remote branch
```bash
git push --force-with-lease origin <branch-name>
```
## 5. Push to two remotes/upstreams
```
git remote set-url --add --push origin <url>
```

- check if works
```
git remote -v
```

- now when you `git push origin main`, you will push to the two urls

## 6. Git Submodules

- if fresh repo (not yet cloned)
```bash
git clone <url> --recurse-submodules
```

- if already existing
```bash
git submodule update --init --recursive
```

- when git pulling
```bash
git pull --recurse-submodules
# or
git pull origin main --recurse-submodules

# can be followed by to update submodules (if above doesn't work)
git submodule update --recursive --remote
```

- when updating existing git submodules to latest remote changes
```bash
git submodule update --recursive --remote
```

- when adding a submodule
```bash
cd <parent-dir-of-submodule>
git submodule add <url> <submodule-dir>

# check if working
git submodule status
```

- using `.gitmodules`
```bash
[submodule "<path-to-submodule-local>"]
    path = <should-be-the-same-as-above-path>
    url = <https-url>
```
