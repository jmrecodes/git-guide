[**&#8592; Back to Table of Contents**](./README.md)

# 4. Branching and Merging

Branching is one of Git's most powerful features. It allows you to diverge from the main line of development and continue to do work without messing with that main line. This is invaluable for developing new features, fixing bugs, or experimenting with new ideas in a safe, isolated environment.

## What is a Branch?

Think of a Git branch as simply a **lightweight movable pointer to one of your commits**. When you start a repository, the default branch is named `main`. As you start making commits, the `main` branch pointer moves forward automatically to point to the last commit you made.

When you create a new branch, all Git needs to do is create a new pointer; it doesn't create a whole new copy of your code. This is why creating and switching between branches in Git is incredibly fast and cheap.

Using branches allows you to:
*   **Work on a new feature** in isolation. If the feature isn't ready, your main codebase remains stable.
*   **Create a "hotfix" branch** to quickly fix a production bug without disrupting your current development work.
*   **Collaborate effectively.** Each developer can work on their own feature branch, and the team can merge the changes later.

## Creating and Switching Branches

Here are the primary commands for working with branches.

*   **To list all branches in your repository:**
    ```/dev/null/branch.sh#L1-1
    git branch
    ```
    The currently active branch will be marked with an asterisk (`*`).

*   **To create a new branch:**
    ```/dev/null/branch.sh#L1-1
    git branch new-feature
    ```
    This creates a new branch called `new-feature`, but it **does not** switch you to it.

*   **To switch to an existing branch:**
    The classic command is `git checkout`:
    ```/dev/null/checkout.sh#L1-1
    git checkout new-feature
    ```
    Modern Git also introduced `git switch`, which is more intuitive and less error-prone because it only deals with branches:
    ```/dev/null/switch.sh#L1-1
    git switch new-feature
    ```

*   **To create a new branch and immediately switch to it:**
    This is a very common workflow, and there's a convenient shortcut for it.
    Using `checkout`:
    ```/dev/null/checkout.sh#L1-1
    git checkout -b new-feature
    ```
    Using `switch`:
    ```/dev/null/switch.sh#L1-1
    git switch -c new-feature
    ```

## Merging Branches

Once you've completed the work on your feature branch, you'll want to integrate it back into your main line of development (e.g., the `main` branch). This process is called **merging**.

The `git merge` command combines the history of two branches.

Here is the typical workflow:
1.  First, make sure the receiving branch (e.g., `main`) is up-to-date.
    ```/dev/null/merge.sh#L1-2
    git switch main
    # git pull origin main (if you're working with a remote)
    ```
2.  Then, run the merge command, specifying the name of the branch you want to merge *in*.
    ```/dev/null/merge.sh#L1-1
    git merge new-feature
    ```
This takes the changes from `new-feature` and applies them to `main`, creating a new "merge commit" that ties the two histories together.

After a successful merge, you can often delete the feature branch since its work is now integrated.
```/dev/null/branch.sh#L1-1
git branch -d new-feature
```

## Resolving Merge Conflicts

Sometimes, Git cannot automatically merge branches. A **merge conflict** occurs when the same line of code has been changed in different ways on both branches. Git will pause the merge process and ask you to resolve the conflict manually.

1.  Run `git status` to see which files have conflicts.
2.  Open the conflicted files in your code editor. Git will have marked the problematic areas like this:
    ```/dev/null/conflict.txt#L1-7
    <<<<<<< HEAD
    This is the content from your current branch (main).
    =======
    This is the content from the branch you are trying to merge (new-feature).
    >>>>>>> new-feature
    ```
3.  You must edit the file to resolve the conflict. This means deleting the markers (`<<<<<<<`, `=======`, `>>>>>>>`) and deciding which version of the code to keep, or combining them in a new way.
4.  Once you've resolved the conflict in the file, save it, and then stage the resolved file:
    ```/dev/null/add.sh#L1-1
    git add path/to/conflicted-file.js
    ```
5.  After staging all resolved files, complete the merge by running:
    ```/dev/null/commit.sh#L1-1
    git commit
    ```
    Git will create the merge commit, and the process is complete.

---

Branching and merging are fundamental to collaborative Git workflows. Next, we'll learn how to share your work with others using remote repositories.

[**Next: Working with Remotes &#8594;**](./working-with-remotes.md)