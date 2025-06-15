[**&#8592; Back to Table of Contents**](./README.md)

# 5. Working with Remotes

So far, everything we've done has been on your local machine. Git is a *distributed* version control system, which means you can connect your local repository to other repositories. These other repositories are called **remotes**.

A remote is typically a version of your project that is hosted on the internet or a network, most commonly on a service like GitHub, GitLab, or Bitbucket.

Working with remotes is how you collaborate with other people. It allows you to share your work and pull in changes from other developers.

## Understanding Remotes

*   **`origin`**: This is the default name Git gives to the remote you cloned from. If you didn't clone a project but instead initialized it locally (`git init`), you won't have an `origin` until you add one.
*   **Remote-tracking branches**: When you fetch from a remote, Git creates special pointers in your local repository to keep track of the state of the branches on that remote. These are called remote-tracking branches and look like `<remote-name>/<branch-name>` (e.g., `origin/main`). You don't edit these branches directly; they update automatically when you communicate with the remote.

## Listing and Adding Remotes

*   **To see which remotes you have configured:**
    Run `git remote -v`. The `-v` (verbose) flag shows the URLs Git has stored for reading (fetch) and writing (push).
    ```/dev/null/remote.sh#L1-1
    git remote -v
    ```
    ```/dev/null/output.txt#L1-2
    origin  https://github.com/user/repo.git (fetch)
    origin  https://github.com/user/repo.git (push)
    ```

*   **To add a new remote repository:**
    You use the `git remote add` command, giving it a short name and the URL.
    ```/dev/null/remote.sh#L1-1
    git remote add <shortname> <url>
    ```
    For example, to add a remote named `upstream` pointing to the original project you forked:
    ```/dev/null/remote.sh#L1-1
    git remote add upstream https://github.com/original-owner/repo.git
    ```

## `git fetch`: Getting Changes from a Remote

The `git fetch` command communicates with a remote and downloads all the commits, files, and branch pointers that you don't have yet.

```/dev/null/fetch.sh#L1-1
git fetch <remote-name>
```
For example, `git fetch origin`.

**Crucially, `git fetch` does not modify your working directory or your local branches.** It only updates the remote-tracking branches (e.g., `origin/main`). This is a safe way to review changes that others have made before you integrate them into your own work.

After fetching, you can compare your local branch with the corresponding remote-tracking branch to see what's new.
```/dev/null/log.sh#L1-1
git log main..origin/main
```

## `git pull`: Fetching and Merging

The `git pull` command is a convenient shortcut. It is essentially a `git fetch` immediately followed by a `git merge`.

```/dev/null/pull.sh#L1-1
git pull <remote-name> <branch-name>
```
For example, if you are on your `main` branch, `git pull origin main` will:
1.  Fetch the latest changes from the `main` branch on the `origin` remote.
2.  Try to merge the `origin/main` branch into your currently checked-out `main` branch.

While convenient, `pull` can be less safe than `fetch` if you're not sure what changes are coming in, as it immediately tries to modify your local branch.

## `git push`: Sharing Your Changes

When you want to share your local commits with others, you use the `git push` command.

```/dev/null/push.sh#L1-1
git push <remote-name> <branch-name>
```

For example:
```/dev/null/push.sh#L1-1
git push origin main
```
This command pushes the commits from your local `main` branch up to the `main` branch on the `origin` remote.

*   **Setting an Upstream Branch:** The first time you push a new local branch, you need to tell Git which remote branch it should track. The `--set-upstream` (or `-u`) flag does this.
    ```/dev/null/push.sh#L1-1
    git push -u origin new-feature
    ```
    This pushes the `new-feature` branch to `origin` and links them. From then on, you can simply run `git push` from that branch, and Git will know where to send the changes.

---

You now have the tools to collaborate with others on a shared codebase. Next, we'll explore some of Git's most powerful (and sometimes feared) commands for rewriting history.

[**Next: Advanced Topics &#8594;**](./advanced-topics.md)