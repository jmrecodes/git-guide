[![hackmd-github-sync-badge](https://hackmd.io/jq7siB9kTICS3zLPuIz5Fg/badge)](https://hackmd.io/jq7siB9kTICS3zLPuIz5Fg)
[**&#8592; Back to Table of Contents**](./README.md)

# 6. Advanced Topics: Rewriting History & More

Welcome to the advanced section of the guide. The commands here are incredibly powerful, but they come with a significant warning: **many of them rewrite commit history**.

Rewriting history can be problematic if you're working on a branch that other people have already pulled from a remote. Changing the history can make it very difficult for others to merge their work. As a rule of thumb, it's generally safe to rewrite history on your own local branches that you haven't shared yet, but you should avoid it on shared branches like `main` or `develop`.

## `git rebase`: Replaying Your Commits

The `git rebase` command is a powerful alternative to `merge`. While `git merge` creates a new merge commit to tie two branches together, `git rebase` takes all the commits from one branch and replays them, one by one, on top of another branch.

The result is a much cleaner, linear history.

**Standard Rebase Workflow:**

1.  You are on your `new-feature` branch, which was created from `main`.
2.  Meanwhile, other developers have added new commits to `main`.
3.  To incorporate their changes and keep your history clean, you can run:
    ```/dev/null/rebase.sh#L1-2
    git switch new-feature
    git rebase main
    ```
This takes your `new-feature` commits, finds the common ancestor with `main`, and replays your commits on top of the latest commit from `main`.

### Interactive Rebase (`git rebase -i`)

This is where `rebase` truly shines. By adding the `-i` (interactive) flag, Git opens an editor with a list of the commits that are about to be replayed, allowing you to edit that list.

For example, `git rebase -i HEAD~3` will open an interactive session for the last three commits. You can then:
*   **`reword`**: Change the commit message.
*   **`edit`**: Stop the rebase at that commit to make changes (e.g., split the commit).
*   **`squash`**: Combine the commit with the previous one, merging the commit messages.
*   **`fixup`**: Like `squash`, but discards the commit's message.
*   **`drop`**: Completely remove the commit.
*   **Reorder lines**: You can change the order of the commits.

Interactive rebase is the ultimate tool for cleaning up your local commit history *before* you share it with others.

## `git reset`: Moving the HEAD Pointer

The `git reset` command is used to unstage files or, more powerfully, to move the `HEAD` pointer to a specific commit, effectively "resetting" your branch. It has three primary modes:

*   **`--soft`**: Moves the `HEAD` pointer but does nothing else. The changes from the "undone" commits are left in your **staging area**.
    ```/dev/null/reset.sh#L1-1
    git reset --soft HEAD~1
    ```
    This undoes the last commit but keeps its changes staged.

*   **`--mixed` (default)**: Moves the `HEAD` pointer and resets the staging area. The changes from the "undone" commits are kept in your **working directory** but are no longer staged.
    ```/dev/null/reset.sh#L1-1
    git reset HEAD~1
    ```

*   **`--hard`**: Moves the `HEAD` pointer, resets the staging area, and resets the working directory. **This is a destructive command.** The changes from the "undone" commits are discarded completely. Use with caution.
    ```/dev/null/reset.sh#L1-1
    git reset --hard HEAD~1
    ```

## `git cherry-pick`: Picking a Specific Commit

Sometimes, you don't want to merge or rebase an entire branch. You just want to grab one specific commit from another branch and apply it to your current branch. This is exactly what `git cherry-pick` is for.

```/dev/null/cherry-pick.sh#L1-1
git cherry-pick <commit-hash>
```

When you do this, Git takes the changes introduced in that specific commit and applies them to your current branch, creating a **brand new commit** with the same changes and a similar commit message.

---

## A Practical Workflow: Combining `cherry-pick` and `reset`

Git's true power is realized when you combine commands to achieve a specific outcome. Here is a brilliant, real-world example of using `cherry-pick` and `reset` together to grab file changes from another branch *without* creating a new commit in your history.

**The Goal:** You want the *file changes* from a single commit on a remote branch, but you want to stage them as if you had made them manually, without bringing the commit itself into your branch history.

**The Steps:**

1.  **Add and Fetch the Remote:** First, you need access to the other branch's history.
    ```/dev/null/remote.sh#L1-2
    git remote add other-repo <url-to-other-repo>
    git fetch other-repo
    ```
2.  **Find the Commit:** Use `git log` to find the hash of the specific commit you want from the remote branch.
    ```/dev/null/log.sh#L1-1
    git log other-repo/main
    ```
3.  **Cherry-Pick the Commit:** This applies the changes to your working directory and, crucially, creates a new commit on your current branch.
    ```/dev/null/cherry-pick.sh#L1-1
    git cherry-pick <commit-hash-you-found>
    ```
    At this point, your branch history has a new commit on top. Your working directory is clean.

4.  **The Clever Move: `git reset --soft`:** This is the key to achieving the goal. You undo the commit you just made, but keep the changes staged.
    ```/dev/null/reset.sh#L1-1
    git reset --soft HEAD~
    ```
    *   `HEAD~` refers to the commit just before your current one (i.e., the state of your branch right before the cherry-pick).
    *   `--soft` tells Git to move the branch pointer back, but to leave the changes from the undone commit neatly in your staging area.

**The Result:**

*   The new commit created by `cherry-pick` is removed from your branch's history.
*   The file modifications from that commit are now in your staging area ("Changes to be committed").
*   Your `git status` shows the files as staged, ready for you to review, modify further, or commit as part of your own work.

This workflow is a perfect illustration of advanced Git usage: using one command (`cherry-pick`) as a tool to get content, and another (`reset --soft`) to manipulate the history to fit your exact needs.