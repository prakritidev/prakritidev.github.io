---
title: '"Git Advanced"'
draft: 
tags:
  - Git
---

### 1. **Advanced Branching and Merging**

#### a. **Branching Strategies**
- **Feature Branches**: Create a separate branch for each feature you're working on, allowing parallel development without affecting the main codebase.
- **Gitflow**: A popular branching strategy that uses multiple branches for different stages of development (e.g., `develop`, `release`, `hotfix`, `master`).

#### b. **Merging Strategies**
- **Fast-Forward Merging**: If the branch you're merging is directly ahead of the branch you're merging into, Git performs a fast-forward merge, updating the pointer to the latest commit.
- **No Fast-Forward (`--no-ff`)**: Ensures that a merge commit is always created, preserving the context of the branch even if it could be fast-forwarded.
  
  ```bash
  git merge --no-ff feature-branch
  ```

- **Squash Merging**: Combines all commits from the branch into a single commit on the target branch.

  ```bash
  git merge --squash feature-branch
  ```

- **Recursive and Ours/Theirs Strategy**: During a merge conflict, you can use the `recursive` strategy with `ours` or `theirs` to resolve conflicts automatically.

  ```bash
  git merge -X ours feature-branch
  git merge -X theirs feature-branch
  ```

### 2. **Rebasing**

#### a. **Interactive Rebase**
Rebasing allows you to modify commit history, clean up commits, or reapply commits on top of another branch.

- **Rebase Interactively**:

  ```bash
  git rebase -i HEAD~3  # Modify the last 3 commits
  ```

  - **Pick**: Use the commit.
  - **Squash**: Combine the commit with the previous one.
  - **Reword**: Edit the commit message.
  - **Drop**: Remove the commit.

#### b. **Rebase vs. Merge**
- **Rebase**: Reapplies commits on top of the base branch, resulting in a linear history.
- **Merge**: Combines changes from two branches, preserving the commit history.

#### c. **Rebasing Onto Another Branch**

- **Rebase Feature Branch Onto `main`**:

  ```bash
  git checkout feature-branch
  git rebase main
  ```

- **Continue Rebase After Conflict**:

  ```bash
  git rebase --continue
  ```

### 3. **Cherry-Picking**

Cherry-picking allows you to apply a specific commit from one branch to another.

- **Apply a Commit**:

  ```bash
  git cherry-pick <commit-hash>
  ```

- **Cherry-Pick Multiple Commits**:

  ```bash
  git cherry-pick <commit-hash-1> <commit-hash-2>
  ```

- **Skip a Commit During Cherry-Picking**:

  ```bash
  git cherry-pick --skip
  ```

### 4. **Stashing**

Stashing is useful when you need to save changes without committing them, allowing you to switch branches or work on something else.

- **Stash Changes**:

  ```bash
  git stash
  ```

- **Apply Stash**:

  ```bash
  git stash apply
  ```

- **List Stashes**:

  ```bash
  git stash list
  ```

- **Apply Specific Stash**:

  ```bash
  git stash apply stash@{1}
  ```

- **Stash with Message**:

  ```bash
  git stash save "Work in progress on feature-x"
  ```

- **Drop a Stash**:

  ```bash
  git stash drop stash@{1}
  ```

### 5. **Git Hooks**

Git hooks are scripts that run automatically in response to specific events in a Git repository. They are useful for enforcing policies, automating tasks, and improving workflow.

#### a. **Types of Hooks**
- **Client-Side Hooks**: Run on your local machine, such as `pre-commit`, `commit-msg`, `pre-push`.
- **Server-Side Hooks**: Run on the remote repository, such as `pre-receive`, `post-receive`.

#### b. **Creating a Hook**
- Navigate to the `.git/hooks` directory and create a file with the name of the hook (e.g., `pre-commit`).
- Write your script (e.g., a Bash script).

  ```bash
  # .git/hooks/pre-commit
  #!/bin/sh
  echo "Running pre-commit hook"
  ```

- Make the hook executable:

  ```bash
  chmod +x .git/hooks/pre-commit
  ```

### 6. **Submodules**

Submodules allow you to include one Git repository as a subdirectory of another repository. This is useful for managing dependencies or incorporating external projects.

#### a. **Adding a Submodule**

```bash
git submodule add https://github.com/example/repo.git path/to/submodule
```

#### b. **Initializing and Updating Submodules**

```bash
git submodule update --init --recursive
```

#### c. **Cloning a Repository with Submodules**

```bash
git clone --recursive https://github.com/your/repo.git
```

#### d. **Removing a Submodule**

```bash
git submodule deinit -f path/to/submodule
git rm -rf path/to/submodule
```

### 7. **Advanced Git Commands**

#### a. **Reflog**
Reflog is used to record updates to the tip of branches and other references. It helps in recovering lost commits.

- **View Reflog**:

  ```bash
  git reflog
  ```

- **Recover a Lost Commit**:

  ```bash
  git checkout <commit-hash-from-reflog>
  ```

#### b. **Bisect**
Git bisect is used to find the commit that introduced a bug by performing a binary search through the commit history.

- **Start Bisect**:

  ```bash
  git bisect start
  ```

- **Mark a Commit as Bad**:

  ```bash
  git bisect bad
  ```

- **Mark a Commit as Good**:

  ```bash
  git bisect good <commit-hash>
  ```

- **Continue Bisecting**:

  - Git will automatically check out a commit. You test it and mark it as either `good` or `bad`.

- **Finish Bisect**:

  ```bash
  git bisect reset
  ```

#### c. **Amending Commits**

- **Amend the Last Commit**:

  ```bash
  git commit --amend
  ```

- **Amend with New Changes**:

  ```bash
  git add <file>
  git commit --amend --no-edit
  ```

- **Amend Commit Message Only**:

  ```bash
  git commit --amend -m "New commit message"
  ```

### 8. **Collaborating with Git**

#### a. **Forking and Pull Requests**
- **Forking**: Create a copy of a repository on your GitHub account.
- **Cloning**: Clone the forked repository to your local machine.
- **Creating a Branch**: Work on a feature in a separate branch.
- **Pushing**: Push your branch to your forked repository.
- **Pull Request**: Submit a pull request to the original repository for review and merging.

#### b. **Rebasing on a Remote Branch**
- **Fetch and Rebase**:

  ```bash
  git fetch origin
  git rebase origin/main
  ```

#### c. **Handling Merge Conflicts**
- **Resolve Conflicts Manually**: Edit conflicting files to resolve issues.
- **Continue After Resolving**:

  ```bash
  git add <resolved-files>
  git rebase --continue
  ```

# Some uncommon commands 

Sure! Here are some advanced Git techniques and lesser-known features that go beyond the basics and are less commonly known, even among experienced Git users:

### 1. **Git Worktrees**

**Git Worktrees** allow you to check out multiple branches at once in different directories, which is useful for working on several features simultaneously without having to constantly switch branches.

- **Create a New Worktree**:

  ```bash
  git worktree add ../branch-directory branch-name
  ```

- **List Existing Worktrees**:

  ```bash
  git worktree list
  ```

- **Remove a Worktree**:

  ```bash
  git worktree remove ../branch-directory
  ```

**Use Case**: If you’re working on a feature but need to start working on a hotfix in parallel, you can check out the hotfix branch in a separate worktree without disturbing your current work.

### 2. **Sparse Checkout**

**Sparse Checkout** is useful when you’re working in a large repository, and you only need a subset of files or directories. Instead of cloning the entire repository, you can clone only the parts you need.

- **Enable Sparse Checkout**:

  ```bash
  git sparse-checkout init --cone
  ```

- **Add Specific Files or Directories**:

  ```bash
  git sparse-checkout set path/to/directory
  ```

**Use Case**: Ideal for monorepos where different teams or projects are managed within the same repository. You can pull only the files relevant to your team, reducing clutter and speeding up operations.

### 3. **Reflog Recovery Techniques**

Reflog records every change in the HEAD, even those that are “lost” after a reset or rebase. You can use Reflog to recover commits that seem lost after a destructive operation like a reset.

- **Recover a Lost Commit**:

  ```bash
  git reflog
  git checkout <commit-hash>
  ```

- **Undo the Last Git Command**:

  ```bash
  git reflog
  git reset --hard HEAD@{1}
  ```

**Use Case**: If you accidentally reset or rebase to the wrong commit, you can use Reflog to recover the original state.

### 4. **Interactive Rebase with Exec**

During an interactive rebase, you can use the `exec` command to run arbitrary commands between commits. This can be useful for automating tests or linters on each commit during a rebase.

- **Interactive Rebase with Exec**:

  ```bash
  git rebase -i HEAD~5
  ```

  Then add the `exec` command to run something between commits:

  ```plaintext
  pick <commit-hash> Commit message
  exec npm test
  pick <commit-hash> Another commit message
  ```

**Use Case**: Ensuring that all your commits pass tests or meet code quality standards during a rebase.

### 5. **Git Bisect with Automation**

**Git Bisect** can be automated with scripts to automatically determine the commit that introduced a bug.

- **Automate Git Bisect**:

  ```bash
  git bisect start
  git bisect bad
  git bisect good <commit-hash>
  git bisect run your-script.sh
  ```

- **Example Script**:

  ```bash
  #!/bin/sh
  make && ./your-test-suite
  ```

**Use Case**: When dealing with large codebases, manually checking each commit during a bisect can be time-consuming. Automating this process saves time and ensures accuracy.

### 6. **Git Attributes for Custom Behavior**

Git Attributes (`.gitattributes`) can be used to customize how Git handles certain files. For example, you can configure Git to handle line endings, perform custom diffing, or even specify custom merge strategies.

- **Force Text File Line Endings to LF**:

  ```bash
  *.sh text eol=lf
  ```

- **Custom Diff for a Specific File Type**:

  ```bash
  *.md diff=markdown
  ```

  Then, define `diff.markdown.textconv` in your Git configuration.

**Use Case**: Ensuring consistent behavior across different operating systems (e.g., Windows vs. Unix) or customizing how Git interprets and handles specific files.

### 7. **Partial/Staged Commits with `patch` Mode**

Git allows you to create partial commits, staging only part of the changes in a file while leaving the rest for later.

- **Stage Part of a File**:

  ```bash
  git add -p
  ```

  Git will present you with a diff, and you can choose which hunks to stage.

- **Split a Hunk**:

  Within the `patch` mode, you can split a hunk into smaller parts to stage only what you need:

  ```plaintext
  s
  ```

**Use Case**: Ideal when you’re working on multiple changes in a single file but want to commit them separately for clarity.

### 8. **Git Notes**

**Git Notes** allow you to add additional information to a commit without modifying the commit itself. This is useful for annotating commits with extra details, links to issue trackers, or code review comments.

- **Add a Note**:

  ```bash
  git notes add -m "This commit fixes issue #123"
  ```

- **Show Notes**:

  ```bash
  git log --show-notes
  ```

- **Push Notes**:

  ```bash
  git push origin refs/notes/commits
  ```

**Use Case**: When you need to add metadata to a commit without altering its history, such as after a review process or to link it to external systems.

### 9. **Advanced Git Aliases**

Creating custom Git aliases can streamline complex or frequently used commands, saving time and reducing the risk of errors.

- **Example Aliases**:

  ```bash
  git config --global alias.co checkout
  git config --global alias.br branch
  git config --global alias.cm "commit -m"
  git config --global alias.st status
  git config --global alias.hist "log --oneline --graph --all --decorate"
  ```

- **Chained Commands**:

  ```bash
  git config --global alias.cleanup "!git fetch --prune && git branch -vv | grep ': gone]' | awk '{print $1}' | xargs -r git branch -d"
  ```

**Use Case**: Creating aliases for repetitive or complex commands makes your workflow more efficient and helps prevent mistakes.

### 10. **Git Subtree**

**Git Subtree** is an alternative to submodules, allowing you to nest one repository inside another. Unlike submodules, subtrees do not require additional work when cloning the repository and are easier to manage.

- **Add a Subtree**:

  ```bash
  git subtree add --prefix=path/to/dir https://github.com/other/repo.git master --squash
  ```

- **Pull Updates into Subtree**:

  ```bash
  git subtree pull --prefix=path/to/dir https://github.com/other/repo.git master --squash
  ```

- **Push Subtree Changes Back**:

  ```bash
  git subtree push --prefix=path/to/dir https://github.com/other/repo.git master
  ```

**Use Case**: Integrating external libraries or shared code into your repository in a way that’s simpler and more robust than using submodules.
