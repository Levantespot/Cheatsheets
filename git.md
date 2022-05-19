# Git & GitHub Tutorial

## Getting Started

Once you have Git on your system, you’ll want to customize your Git environment. You should have to do these things only once on any given computer; they’ll stick around between upgrades. You can also change them at any time by **re-running** through the commands.

Git comes with a tool called `git config` that lets you get and set configuration variables that control all aspects of how Git looks and operates. These variables can be stored in three different places:

1. `[path]/etc/gitconfig` file: Contains values applied to every user on the system and all their repositories. If you pass the option `--system` to `git config`, it reads and writes from this file specifically. Because this is a system configuration file, you would need administrative or superuser privilege to make changes to it.
2. `~/.gitconfig` or `~/.config/git/config` file: Values specific personally to you, the user. You can make Git read and write to this file specifically by passing the `--global` option, and this affects *all* of the repositories you work with on your system.
3. `config` file in the Git directory (that is, `.git/config`) of whatever repository you’re currently using: Specific to that single repository. You can force Git to read from and write to this file with the `--local` option, but that is in fact the default. Unsurprisingly, you need to be located somewhere in a Git repository for this option to work properly.

### Your Identity

The first thing you should do when you install Git is to set your user name and email address. This is important because every Git commit uses this information, and it’s immutably baked into the commits you start creating:

```shell
$ git config --global user.name "John Doe"
$ git config --global user.email johndoe@example.com
```

### Your Editor

Now that your identity is set up, you can configure the default text editor that will be used when Git needs you to type in a message. If not configured, Git uses your system’s default editor.

If you want to use a different text editor, such as Vim, you can do the following:

```shell
$ git config --global core.editor vim
```

On a Windows system, if you want to use a different text editor, you must specify the full path to its executable file. This can be different depending on how your editor is packaged.

```shell
$ git config --global core.editor "'C:/Program Files/Notepad++/notepad++.exe' -multiInst -notabbar -nosession -noPlugin"
```

### Your default branch name

By default Git will create a branch called *master* when you create a new repository with `git init`. From Git version 2.28 onwards, you can set a different name for the initial branch.

To set *main* as the default branch name do:

```shell
$ git config --global init.defaultBranch main
```

### Checking Your Settings

If you want to check your configuration settings, you can use the `git config --list` command to list all the settings.

You may see keys more than once, because Git reads the same key from different files (`[path]/etc/gitconfig` and `~/.gitconfig`, for example). In this case, Git uses the last value for each unique key it sees.

You can also check what Git thinks a specific key’s value is by typing `git config <key>`:

```shell
$ git config user.name
John Doe
```

### Getting Help

If you ever need help while using Git, there are three equivalent ways to get the comprehensive manual page (manpage) help for any of the Git commands:

```shell
$ git help <verb>
$ git <verb> --help
$ man git-<verb>
```

For example, you can get the manpage help for the `git config` command by running this:

```shell
$ git help config
```

In addition, if you don’t need the full-blown manpage help, but just need a quick refresher on the available options for a Git command, you can ask for the more concise “help” output with the `-h` option.

## Git Basics

### Create a local git repository 

```shell
$ cd <somewhere>
$ git init
```

This creates a new subdirectory named `.git` that contains all of your necessary repository files — a Git repository skeleton. At this point, nothing in your project is tracked yet. 

If you want to start version-controlling existing files (as opposed to an empty directory), you should probably begin tracking those files and do an initial commit. You can accomplish that with a few `git add` commands that specify the files you want to track, followed by a `git commit`:

```shell
$ git add *.c
$ git add LICENSE
$ git commit -m 'Initial project version'
```

### Cloning an Existing Repository

You clone a repository with `git clone <url>`. For example, if you want to clone the Git linkable library called `libgit2`, you can do so like this:

```shell
$ git clone https://github.com/libgit2/libgit2
```

That creates a directory named `libgit2`, initializes a `.git` directory inside it, pulls down all the data for that repository, and checks out a working copy of the latest version. If you go into the new `libgit2` directory that was just created, you’ll see the project files in there, ready to be worked on or used.

If you want to clone the repository into a directory named something other than `libgit2`, you can specify the new directory name as an additional argument:

```shell
$ git clone https://github.com/libgit2/libgit2 mylibgit
```

Git has a number of different transfer protocols you can use. The previous example uses the `https://` protocol, but you may also see `git://` or `user@server:path/to/repo.git`, which uses the SSH transfer protocol.

### Recording Changes to the Repository

![The lifecycle of the status of your files](git.assets/lifecycle.png)

**Checking** the Status of Your Files

```shell
$ git status
$ E:\Study\Cheatsheets> git status
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   git.md

no changes added to commit (use "git add" and/or "git commit -a")
```

**Tracking** New Files

In order to begin tracking a new file, you use the command `git add`.

```shell
$ git add <file>...
```

**Staging** Modified Files

To stage it, you run the `git add` command. `git add` is a multipurpose command — you use it to begin tracking new files, to stage files, and to do other things like marking merge-conflicted files as resolved.

| Command                    | New Files | Modified Files | Deleted Files | Description                                                |
| :------------------------- | :-------- | :------------- | :------------ | :--------------------------------------------------------- |
| git add -A                 | ✔️         | ✔️              | ✔️             | Stage all (new, modified, deleted) files                   |
| git add .                  | ✔️         | ✔️              | ✔️             | Stage all (new, modified, deleted) files in current folder |
| git add --ignore-removal . | ✔️         | ✔️              | ❌             | Stage new and modified files only                          |
| git add -u                 | ❌         | ✔️              | ✔️             | Stage modified and deleted files only                      |

**Ignoring** Files

Often, you’ll have a class of files that you don’t want Git to automatically add or even show you as being untracked. These are generally automatically generated files such as log files or files produced by your build system. In such cases, you can create a file listing patterns to match them named `.gitignore`. Here is an example `.gitignore` file:

```shell
$ cat .gitignore
*.[oa]
*~
```

The first line tells Git to ignore any files ending in “.o” or “.a” — object and archive files that may be the product of building your code. The second line tells Git to ignore all files whose names end with a tilde (`~`), which is used by many text editors such as Emacs to mark temporary files.

The rules for the patterns you can put in the `.gitignore` file are as follows:

- Blank lines or lines starting with `#` are ignored.
- Standard glob patterns work, and will be applied recursively throughout the entire working tree.
- You can start patterns with a forward slash (`/`) to avoid recursivity.
- You can end patterns with a forward slash (`/`) to specify a directory.
- You can negate a pattern by starting it with an exclamation point (`!`).

**Committing** Your Changes

Now that your staging area is set up the way you want it, you can commit your changes. Remember that anything that is still unstaged — any files you have created or modified that you haven’t run `git add` on since you edited them — won’t go into this commit. They will stay as modified files on your disk.

```shell
$ git commit -m "Message"
```

**Skipping** the Staging Area

Adding the `-a` option to the `git commit` command makes Git automatically stage every file that is already tracked before doing the commit, letting you skip the `git add` part:

```shell
$ git commit -a -m 'Message'
```

**Removing** Files

To remove a file from Git, you have to remove it from your tracked files (more accurately, remove it from your staging area) and then commit. The `git rm` command does that, and also removes the file from your working directory so you don’t see it as an untracked file the next time around.

If you simply remove the file from your working directory, it shows up under the “Changes not staged for commit” (that is, *unstaged*) area of your `git status` output.

Another useful thing you may want to do is to keep the file in your working tree but remove it from your staging area. In other words, you may want to keep the file on your hard drive but not have Git track it anymore. This is particularly useful if you forgot to add something to your `.gitignore` file and accidentally staged it, like a large log file or a bunch of `.a` compiled files. To do this, use the `--cached` option:

```shell
$ git rm --cached README
```

**Moving** Files

```shell
$ git mv file_from file_to
```

## Git Branching

### Local branches

**List** all of the branches in your repository:

```shell
$ git branch
```

**Create** a new branch called `＜branch＞`: This does *not* check out the new branch.

```shell
$ git branch <branch>
```

**Switch** to or check out the new branch

```shell
$ git checkout <branch>

$ git checkout -b iss53 # Switched to a new branch "iss53"
# This is shorthand for: "git branch iss53" and "git checkout iss53"
```

**Delete** the specified branch:

```shell
$ git branch -d <branch> # This is a “safe” operation in that Git prevents you from deleting the branch if it has unmerged changes:
$ git branch -D <branch> # Force delete the specified branch, even if it has unmerged changes.
```

**Rename** the current branch to `＜branch＞`:

```shell
$ git branch -m <branch>
```

**Merge** a branch to current branch:

```shell
$ git merge <branch>
```

### Remote branches

In order to operate on remote branches, a remote repo must first be configured and added to the local repo config. 

**Adding** a remote repository:

```shell
$ git remote add <remote_name> <remote_url>
```

**List** all remote branches:

```shell
$ git branch -a
```

**Delete** a remote branch:

```shell
$ git push <remote_name> --delete <branch_name> # this will not delete local branch

# or delete local branch first then push to remote repository
$ git branch -D <branch_name>
$ git push <remote_name> <branch_name>
```

## Git on the server

### Generating Your SSH Public Key

First, you should check to make sure you don’t already have a key. By default, a user’s SSH keys are stored in that user’s `~/.ssh` directory.

```shell
$ cd ~/.ssh
$ ls
```

You’re looking for a pair of files named something like `id_dsa` or `id_rsa` and a matching file with a `.pub` extension. The `.pub` file is your public key, and the other file is the corresponding private key. If you don’t have these files (or you don’t even have a `.ssh` directory), you can create them by running a program called `ssh-keygen`.

```shell
$ ssh-keygen -o
```

First it confirms where you want to save the key (`.ssh/id_rsa`), and then it asks twice for a passphrase, which you can leave empty if you don’t want to type a password when you use the key. However, if you do use a password, make sure to add the `-o` option; it saves the private key in a format that is more resistant to brute-force password cracking than is the default format. You can also use the `ssh-agent` tool to prevent having to enter the password each time.

## GitHub

### Work for your repository

Generate a SSH key. [Link](#Generating Your SSH Public Key)

```shell
$ cat ~/.ssh/id_rsa.pub
```

Then paste the contents of your `~/.ssh/id_rsa.pub` public-key file into the "Add an SSH Key" text area, and click “Add key”.

Next, specifies the remote repository for your local repository.

```shell
$ cd workdir
$ git init # use for the first time
$ git remote add <remote_name> <remote_url>
$ git remote add origin git@github.com:Levantespot/Cheatsheets.git
```

Updates your current local working directory. `git pull` is a combination of `git fetch` and `git merge`

```shell
$ git pull <remote_name> <branch>
$ git pull origin main # Or `$ git pull origin main:main` which will create a `main` branch or merge remote rep with your `main` branch
```

Create a `main` branch

```shell
$ git checkout -b main
```

This is because GitHub rename the master to main

Push

```shell
$ git add .
$ git commit -m "message"
$ git push origin main
```



### Work cooperatively

TODO: Fork & Pull