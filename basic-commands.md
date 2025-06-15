[![hackmd-github-sync-badge](https://hackmd.io/54B2bKw5SViJD3hlrmGAoQ/badge)](https://hackmd.io/54B2bKw5SViJD3hlrmGAoQ)
[**&#8592; Back to Table of Contents**](./README.md)

# 3. Basic Commands: The Day-to-Day Workflow

This section covers the essential commands you will use constantly. Mastering this workflow is the key to using Git effectively.

## The Three States of a File

To understand the basic commands, you must first understand the three main states a file can be in within Git:

1.  **Working Directory:** This is your local project directory. Any file you create, edit, or delete here is in the working directory. These changes are not yet part of Git's history.
2.  **Staging Area (or Index):** This is an intermediate area where you prepare ("stage") changes for the next commit. It acts as a draft space, allowing you to select which modifications from your working directory you want to include in the next snapshot.
3.  **Repository (`.git` directory):** This is where Git permanently stores your committed snapshots. Once a change is committed, it has a unique ID and is safely stored in your local database.

The basic workflow moves changes from the Working Directory to the Staging Area, and then from the Staging Area to the Repository.

---

## `git status`: Checking Your Project's Status

This is the most-used command in Git. It tells you the current state of your working directory and staging area, showing which files are new, modified, or staged.

```/dev/null/status.sh#L1-1
git status
```

The output will tell you:
*   Which branch you are currently on.
*   If your branch is up to date with a remote branch.
*   Files that have been modified but not yet staged ("Changes not staged for commit").
*   Files that are staged and ready to be committed ("Changes to be committed").
*   New files that Git doesn't know about yet ("Untracked files").

## `git add`: Staging Changes

The `git add` command moves changes from your working directory to the staging area.

*   **To stage a single file:**
    ```/dev/null/add.sh#L1-1
    git add filename.txt
    ```

*   **To stage multiple files:**
    ```/dev/null/add.sh#L1-1
    git add file1.html file2.css
    ```

*   **To stage all changes (new files and modifications) in the current directory and subdirectories:**
    ```/dev/null/add.sh#L1-1
    git add .
    ```
    The `.` represents the current directory. This is a very common and convenient way to stage all your work.

## `git commit`: Saving Your Snapshot

The `git commit` command takes everything from the staging area and saves it as a new snapshot in your repository's history.

To commit your staged changes, you run:
```/dev/null/commit.sh#L1-1
git commit -m "Your descriptive commit message"
```
The `-m` flag allows you to provide a commit message directly on the command line. A good commit message is crucial. It should be a concise summary of the changes you made. A common convention is to write a short summary (50 characters or less) in the present tense (e.g., "Add user authentication feature" instead of "Added...").

If you run `git commit` without `-m`, Git will open your configured text editor to let you write a more detailed, multi-line commit message.

## `git log`: Viewing the Commit History

The `git log` command displays the history of commits for the current branch.

```/dev/null/log.sh#L1-1
git log
```

By default, it shows:
*   The unique **commit hash** (a SHA-1 checksum).
*   The **author** (name and email).
*   The **date** of the commit.
*   The **commit message**.

`git log` has many options to format the output. A popular and useful one is:
```/dev/null/log.sh#L1-1
git log --oneline --graph --decorate
```
*   `--oneline`: Condenses each commit to a single line.
*   `--graph`: Displays an ASCII art graph of the branch and merge history.
*   `--decorate`: Shows branch pointers and other references.

## `.gitignore`: Ignoring Files

Often, you have files or directories in your project that you do not want to track with Git. These can include build artifacts, log files, or files with sensitive information.

You can tell Git to ignore these files by creating a file named `.gitignore` in the root of your repository.

Each line in the `.gitignore` file specifies a pattern.
*   Blank lines or lines starting with `#` are ignored.
*   Simple filenames ignore any file with that name, anywhere in the repo.
*   Using `/` at the beginning of a pattern anchors it to the root directory.
*   Using `/` at the end of a pattern specifies a directory.
*   Wildcards (`*`) can be used to match multiple characters.

**Example `.gitignore` file:**
```/dev/null/.gitignore#L1-6
# Ignore dependency directories
node_modules/
vendor/

# Ignore log files
*.log
/debug.log
```
It is a best practice to create the `.gitignore` file early in your project's life and commit it to the repository so that the rules are shared with all collaborators.

---

With these commands, you can manage the entire lifecycle of your project's changes. Next, we'll explore how to manage different lines of development using branches.

[**Next: Branching and Merging &#8594;**](./branching-and-merging.md)