---
title: How to Combine Multiple Repositories into One (and Preserve History)
categories: 
tags: [git]
---

## The Why

At the start of my first year of CS education I was still trying to quite grasp my head around the basics of git. For this reason, every small exercise that I did for my introductory OOP class was designated its own repository, so I could play with the tool and familiarize myself with it (or so I thought).

This resulted in me having too many repos cluttering my GitHub and disrupting my inner peace and balance. Something had to be done with this unacceptable mess, I said to myself.

I wanted to **combine these repositories into one** containing all of them as subdirectories, while also **preserving commit history** of every one of the many original repos. Here I'm going to show how to do it.

We're going to go from this:

```
── project_1
   ├── .git
   ├── .gitignore
   ├── some_file.txt
   └── some_directory
── project_2
   ├── .git
   ├── .gitignore
   ├── some_file.txt
   └── some_directory
── project_3
   ├── .git
   ├── .gitignore
   ├── some_file.txt
   └── some_directory
...
── project_n
   ├── .git
   ├── .gitignore
   ├── some_file.txt
   └── some_directory
```

...to this:

```
── one_big_repository
   ├── .git
   ├── .gitignore
   ├── project_1
   |   ├── some_file.txt
   |   └── some_directory
   ├── project_2
   |   ├── some_file.txt
   |   └── some_directory
   ├── project_3
   |   ├── some_file.txt
   |   └── some_directory
   |   ...
   └── project_n
       ├── some_file.txt
       └── some_directory
```

> This guide assumes you are using macOS or a Linux distribution and are familiar with the basics of navigating files in the command line (`ls`, `cd`, `mkdir`). If you're using Windows you will need to use [Windows-specific versions of Linux commands](https://www.geeksforgeeks.org/linux-vs-windows-commands/){:target="_blank"}.
{: .prompt-info }

## The How

### 1. Create a new repository

First initialize a new git repository by opening a terminal, navigating inside a folder where you want to create a target repo and typing:

```terminal
$ git init
```

You also **have to make at least one commit** to the target repository before merging, for example with a `README.md`{: .filepath} file (or any other files such as `.gitignore`{: .filepath}, etc.), otherwise you won't be able to merge branches together.

```terminal
$ echo "This is a combined repository" > README.md
$ git add README.md
$ git commit -m "Initial commit"
```

### 2. Copy (`fetch`) files to the target repo

> From this point forward in this guide, you will need to repeat the sequence of commands from steps 2-N for each repository which you want to include in the final merged repository.
{: .prompt-warning }

First, add a to-be-merged repository as a remote, either specifying an absolute path on your local machine or a URL to a repository on an external server (like GitHub):

```terminal
$ git remote add project_1_repo /absolute/path/to/project_1
```

Fetch the files to the target repo and add a reference to them by creating a local branch tracking the `main` remote branch:
> Keep in mind that in this step only the commit history of the `main` branch will be preserved. If you have diverging branches in the `project_1`{: .filepath} repo and want to maintain their commit history, before this step you have to merge them into `main`.

```terminal
$ git fetch project_1_repo --tags
$ git checkout -b project_1_branch project_1_repo/main
```

> Throughout the guide I'm using `main` as the primary branch in all of the repositories. If yours are called `master` or something else, modify the commands accordingly. (You can type `git branch` to display the names of all branches in the repository)
{: .prompt-info }

After getting the files in the target repo you can safely remove the link to the remote:

```terminal
$ git remote remove project_1_repo
```

### 3. Move files to a separate directory

Create a new directory:

```terminal
$ mkdir project_1
```

...and move there all the necessary files which belonged to the `project_1`{: .filepath} repository:

```terminal
$ git mv some_file.txt some_directory project_1
```

> When using the `git mv` command you have to include the files and directories you want to move as space-separated arguments, with the destination directory as the last argument (`git mv <source> <source> ... <source> <destination>`). If a file has spaces within its name, surround it with double quotes. Example: `git mv file_1.txt "file with spaces in the name.txt" "another file.txt" destination_directory`
{: .prompt-info }

Now save the changes as a commit (so that they can later be merged into the `main` branch):

```terminal
$ git commit -m "Add project_1 directory"
```

### 4. Merge changes into `main`

Finally, merge the added directory into the `main` branch.

```terminal
$ git checkout main
$ git merge --allow-unrelated-histories project-1-branch
```

The flag `--allow-unrelated-histories` is needed because `git` by default won't let you merge branches which don't share at least one commit in their histories.

To clean up, you can now remove the project branch:

```terminal
git branch -d project_1_branch
```