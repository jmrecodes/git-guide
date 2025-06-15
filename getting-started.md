[**&#8592; Back to Table of Contents**](./README.md)

# 2. Getting Started with Git

This section will walk you through installing Git on your computer, performing the essential initial configuration, and creating your very first repository.

## Installing Git

Git is freely available for all major operating systems.

*   **macOS:** The easiest way to install Git is via [Homebrew](https://brew.sh/). If you have Homebrew, simply open your terminal and run:
    ```/dev/null/install.sh#L1-1
    brew install git
    ```
    Alternatively, you can install it as part of the Xcode Command Line Tools.

*   **Windows:** The official build can be downloaded from the [Git for Windows](https://git-scm.com/download/win) website. This package includes both the command-line tool and a graphical user interface (Git GUI).

*   **Linux:** You can typically install Git through your distribution's package manager.
    *   For Debian/Ubuntu-based distributions:
        ```/dev/null/install.sh#L1-1
        sudo apt-get install git
        ```
    *   For Fedora/CentOS/RHEL:
        ```/dev/null/install.sh#L1-1
        sudo yum install git
        ```

To verify that Git was installed correctly, you can open your terminal (or Git Bash on Windows) and run:

```/dev/null/verify.sh#L1-1
git --version
```

This command should return the version of Git you have installed, for example `git version 2.37.1`.

## First-Time Git Configuration

Before you start using Git, you need to set your user name and email address. This is important because every Git commit uses this information, and it’s immutably baked into the commits you start creating.

Run the following commands in your terminal, replacing the example text with your own name and email.

```/dev/null/config.sh#L1-2
git config --global user.name "Your Name"
git config --global user.email "youremail@example.com"
```

The `--global` flag tells Git that you want to use this configuration for every project you work on. If you need to use a different name or email for a specific project, you can run the same command *without* the `--global` flag from inside that project's directory.

It's also a good practice to set the name of the default branch. Historically, this branch was named `master`, but the community standard is now `main`.

```/dev/null/config.sh#L1-1
git config --global init.defaultBranch main
```

To check your configuration settings, you can use:

```/dev/null/config.sh#L1-1
git config --list
```

## Creating Your First Repository

A **repository** (or "repo") is a directory where Git tracks all the files and folders of your project.

To create a new repository, follow these steps:

1.  Create a new directory for your project and navigate into it.
    ```/dev/null/mkdir.sh#L1-2
    mkdir my-first-project
    cd my-first-project
    ```

2.  Initialize a Git repository.
    ```/dev/null/init.sh#L1-1
    git init
    ```

Git will respond with something like: `Initialized empty Git repository in /path/to/my-first-project/.git/`

This command creates a hidden subdirectory named `.git` that contains all of your repository's history and metadata. This `.git` folder is the heart of your repository.

And that's it! You now have a fully functional Git repository, ready to start tracking your project's history.

---

Now that your repository is set up, let's learn the basic commands you'll use every day.

[**Next: Basic Commands &#8594;**](./basic-commands.md)