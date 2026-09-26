# 🛠️ Git & GitHub Cheat Sheet

[← Back to Main Index](./README.md "Main Index")

Your go-to guide for local staging, branch management, remote syncing, and undoing common mistakes.

---

## ⚙️ Setup & Configuration

| Command | Action |
| :--- | :--- |
| `git config --global user.name "<name>"` | Set the name attached to your commits. |
| `git config --global user.email "<email>"` | Set the email attached to your commits. |
| `git config --global init.defaultBranch main` | Use `main` as the default branch for new repositories. |
| `git config --global pull.rebase true` | Rebase instead of merge when running `git pull`. |
| `git config --global fetch.prune true` | Automatically remove deleted remote branches on fetch. |
| `git config --list --show-origin` | Show every config value and the file it comes from. |

<details>
<summary>🔍 Handy <code>~/.gitconfig</code> template...</summary>

```ini
[user]
    name = Your Name
    email = you@example.com
[init]
    defaultBranch = main
[pull]
    rebase = true
[fetch]
    prune = true
[push]
    autoSetupRemote = true   # first `git push` creates the upstream automatically
[rebase]
    autoStash = true
[alias]
    st = status -sb
    co = checkout
    sw = switch
    lg = log --oneline --graph --decorate --all
    last = log -1 HEAD --stat
    undo = reset --soft HEAD~1
```
</details>

---

## 📦 Local Staging & Changes

| Command | Action |
| :--- | :--- |
| `git init` | Initialize a brand new local Git repository. |
| `git clone <URL>` | Copy a remote repository to your machine. |
| `git status -sb` | Show a short summary of the working directory and staging area. |
| `git add .` | Stage all modified, new, and deleted files in the current directory. |
| `git add -p <file>` | Interactively review and stage changes hunk by hunk. |
| `git commit -m "message"` | Commit staged changes with a descriptive message. |
| `git commit --amend` | Modify the most recent commit (add staged files or edit message). |
| `git commit --amend --no-edit` | Add staged changes to the last commit without changing its message. |

---

## 🌿 Branching & Merging

| Command | Action |
| :--- | :--- |
| `git branch -a` | List all local and remote tracking branches. |
| `git switch -c <name>` | Create a new local branch and switch to it (same as `git checkout -b`). |
| `git switch <branch>` | Switch to an existing branch. |
| `git switch -` | Switch back to the previously checked-out branch. |
| `git merge <branch>` | Merge the specified branch into your current branch. |
| `git merge --abort` | Cancel a merge that has conflicts and return to the pre-merge state. |
| `git cherry-pick <commit_hash>` | Apply a single commit from another branch onto the current one. |
| `git branch -m <new_name>` | Rename the current branch. |
| `git branch -d <branch>` | Safely delete a local branch that has been fully merged. |
| `git branch -D <branch>` | Force-delete a local branch, ignoring its merge status. |

---

## ✂️ Rebasing & Rewriting History

> ⚠️ Only rewrite commits that haven't been pushed to a shared branch.

| Command | Action |
| :--- | :--- |
| `git rebase <branch>` | Replay your current branch's commits on top of `<branch>`. |
| `git rebase -i HEAD~<n>` | Interactively edit, reorder, squash, or drop the last `n` commits. |
| `git rebase --continue` | Continue a rebase after resolving conflicts. |
| `git rebase --abort` | Cancel the rebase and restore the branch to its original state. |
| `git commit --fixup <commit_hash>` | Create a commit that will be squashed into `<commit_hash>`… |
| `git rebase -i --autosquash <base>` | …then fold all fixup commits in automatically. |
| `git push --force-with-lease` | Force-push rewritten history, but refuse if someone else pushed first. |

<details>
<summary>🔍 Interactive rebase keywords...</summary>

| Keyword | Effect |
| :--- | :--- |
| `pick` | Keep the commit as-is. |
| `reword` | Keep the commit, but edit its message. |
| `edit` | Pause at the commit so you can amend it. |
| `squash` | Merge into the previous commit and combine messages. |
| `fixup` | Merge into the previous commit and discard this message. |
| `drop` | Remove the commit entirely. |
</details>

---

## 🌐 Remote Repositories

| Command | Action |
| :--- | :--- |
| `git remote -v` | List configured remotes and their URLs. |
| `git remote add origin <URL>` | Link your local repository to a remote repository URL. |
| `git remote set-url origin <URL>` | Change the URL of an existing remote. |
| `git fetch --all --prune` | Download updates from all remotes and drop deleted remote branches. |
| `git pull origin <branch>` | Fetch changes from the remote branch and merge them locally. |
| `git pull --rebase` | Fetch and rebase your local commits on top of the remote ones. |
| `git push -u origin <branch>` | Push a new branch and set it as the upstream for future pushes. |
| `git push origin <branch>` | Upload your local branch commits to the remote repository. |
| `git push origin --delete <branch>` | Delete a branch directly from the remote server. |

---

## 🏷️ Tags

| Command | Action |
| :--- | :--- |
| `git tag` | List all tags. |
| `git tag -a v1.0.0 -m "message"` | Create an annotated tag on the current commit. |
| `git push origin v1.0.0` | Push a single tag to the remote. |
| `git push origin --tags` | Push all local tags to the remote. |
| `git tag -d v1.0.0` | Delete a local tag. |

---

## 🔍 History & Inspection

| Command | Action |
| :--- | :--- |
| `git log --oneline --graph --all` | View a condensed, visual timeline of all branches. |
| `git log -p <file>` | Show the full change history of a single file. |
| `git log -S "text"` | Find commits that added or removed a specific string. |
| `git log main..<branch>` | Show commits on `<branch>` that aren't on `main`. |
| `git diff` | Show unstaged changes in the working directory. |
| `git diff --staged` | Show changes between your staged files and the last commit. |
| `git diff <branch1>...<branch2>` | Show what `<branch2>` changed since it diverged from `<branch1>`. |
| `git blame <file>` | Show who modified each line of a file and when it happened. |
| `git show <commit_hash>` | See the full metadata and content changes of a specific commit. |

---

## 🚨 Undo Operations & Recovery

| Command | Action |
| :--- | :--- |
| `git restore <file>` | Discard uncommitted changes made to a file in your workspace. |
| `git restore --staged <file>` | Unstage a file but keep its changes in your workspace. |
| `git reset --soft HEAD~1` | Undo the last commit, but keep all its changes staged. |
| `git reset HEAD~1` | Undo the last commit and keep its changes unstaged (default `--mixed`). |
| `git reset --hard HEAD~1` | Completely wipe out the last commit and all changes made in it. |
| `git reset --hard origin/<branch>` | Throw away local work and match the remote branch exactly. |
| `git revert <commit_hash>` | Create a new commit that safely rolls back a previous commit's code. |
| `git stash` | Temporarily shelve uncommitted work to give you a clean directory. |
| `git stash -u` | Stash including untracked files. |
| `git stash list` | List all stashes. |
| `git stash pop` | Reapply your most recent stash and remove it from the list. |
| `git stash apply stash@{n}` | Reapply a specific stash but keep it in the list. |
| `git clean -fd` | Delete all untracked files and directories (preview with `-n` first). |
| `git reflog` | View a log of all HEAD movements to find "lost" commits. |
| `git branch <name> <commit_hash>` | Recover a deleted branch from a hash found in the reflog. |
