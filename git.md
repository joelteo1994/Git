# Git Cheatsheet: Core Workflow & Commands 
This is your personal Git + Github command reference for collaborating on projects, working with branches and managing version control 
- Git is a distributed version control system that helps track code changes, collaboration amongst developers, and experiment safely without fear of losing work. It lets you: 
    - Save snapshots of project over time 
    - Create separate lines of development (branches)
    - Revert to previous states if needed 
    - Merge work with other teammates 
    - Work offline and sync later 

- Github is a cloud-based platform that hosts Git repositories online, making it easy to collaborate with others, review code, open issues, and deploy projects. It adds powerful features on top of Git, like 
    - Pull requests for code reviews 
    - Issues and Discussions for team collab 
    - Visual commit history and differences 
    - Continuous integration (CI/CD)
    - Project boards, wikis, and pages

- Essentially, Github is built on top of Git, adding powerful cloud-based features to make collab, review and project management easier 
    - Git is the local engine for version control: offline, decentralised but only restricted to own machine, whereas Github is an online, centralised collab hub 
    - Git is a standalone command-line program, written in C and not Python. 
        - Runs directly in terminal (bash, zsh etc) and is installed on my system 
    - I can use Git commands to tell Git to connect to Github to push or pull code 
-------
## Step 1: Common Git Setup Config 
Before making commits, should tell Git who you are. This information is stored locally and used to tag your commits. 
To do so, run these commands once on my machine, if (i) using Git for the first time, (ii) am setting up a new laptop, or (iii) want to change my Git identity. Once set, Git saves it to a config file so every Git repo and every terminal session on my machine will use it automatically. 
- git config --global user.name "Joel Teo" (sets my name for all Git commits made on my machine)
- git config --global user.email "joelteo1994@hotmail.com" (sets my email for all Git commits made on my machine)
    - Note that the user.email should match my Github email, but user.name can be anything I want 
    - This is since Github uses the email in my Git commits to 
        - Link my commits to my Github account, and track my contributions properly
        - Check the right email by retrieving from the Github account settings page 
- git config --global --list (run this after setting configurations to check what I have got down)

## Step 2a: Connect Git to Github (if starting new repo from scratch)
Idea here is to (1) create a new repo on Github, (2) link it to my local project using git remote add origin, (3) push local code to Github 
- Create Github repo but do not initialise with README, .gitignore, or license. 
- Create new Git Repo locally that you want to push to Github 
    - Create new project folder by 
        - mkdir folder_name 
        - cd folder_name 
    - If folder exists already, just cd into it 
- Initialise the Git repo locally by 
    - Running git init 
    - This creates a hidden .git folder which Git tracks 
- Add files and make the first commit 
    - git add . (stages/notes changes of all files)
    - git commit -m "Initial commit" (creates snapshot of all changes)
- Link local repo to Github by using the HTTPS URL of the Github repo I just created, and running 
    - git remote add origin "github_https_url"
        - This sets up origin as the nickname for the remote Github repo 
        - Confirm connection with git remote-v 
- Push code to Github by using 
    - git push -u origin main 
        - -u sets upstream tracking so can just use git push in future 
        - if main does not exist, can use git branch -M main

## Step 2b: If repo already exists on Github, i just clone it, no need gor git init
- If there is already a repo on Github that you want to work on, just do 
    - git clone "github_https_url". This does three things automatically 
        - Downloads all the files in the repo 
        - initialises git for you (no need for another git init)
        - sets remote as origin 
- Then, just cd repo-name, git status and start coding!
    - git status is dashboard that gives live snapshot of what's going on in repo right now. Tells you 
        - Which branch I am on 
        - What files have changed
        - What files are staged (ready to commit)
        - What files are not staged yet 
        - What files are untracked (new files Git dosen't know about)

## Step 3: Typical workflow once the repo is already set-up 
Once the Github repo is connected, your main workflow will revolve around developing features or fixes on isolated branches, then merging them safely back into main. This approach is preferred because it:  
- Keeps main stable and production ready 
- Allows for code review and discussion via Pull Requests (PRs)
- Prevents conflicts or broken code from affecting other developers
#### Git Feature Branch Workflow
Main terminal functions to enable this workflow on git  are: 
- git status (to see what files have changed - added, deleted, modified)
- git checkout -b desired_branch_name (create and switch to a new branch from main; checkout switches to that branch and -b creates it)
- git add . (stage all changes I made)
    - use . for everything, or specify files by writing the file name
- git commit -m "..." (commit them with short but clear and technical descriptive message that focuses on what changed)
    - can be <type>: <short summary>
    - e.g. add sentiment analysis module, fix bug in table extraction, refactor section parser logic 
- git push -u origin desired_branch_name (pushes the new branch to Github and set it to track that branch for future git push)
    - TLDR: "Create the branch on GitHub if it doesn’t exist, and remember that this branch is tied to it from now on."
    - Then in future can just do git push and git pull 
    - Note: must explicitly run -u the first time to create tracking. After that, git pull knows which remote branch to sync with 
        - Run git branch -vv to see what are the local/remote pairs established 
        - "-u" is a flag that establishes a local tracking relationship between my branch and the remote branch
            - shorthand for --set-upstream 
#### Then, go to Github to 
- Navigate to repository 
- Will see a Compare & Pull Request banner - click it 
- Add title and clear description of what the PR does 
- Click Create Pull Request
- Once reviewed by teammate, click Merge Pull Request to integrate into main 
#### Once PR is merged
Once the PR is merged on Github, the remote main branch has new commits
However, the local main branch still dosen't know about them yet; it is outdated. To fix this, run on my terminal
- git checkout main (switch back to the main branch)
- git pull origin main (to get the latest merged code downloaded on my local machine)
- git branch -d desired_branch_name (delete the local feature branch)
- git push origin --delete desired_branch_name
    
## Important Note about version discrepancies between local and remote
- You can only push stuff from local to remote if the local version is at least as updated as the remote. If not will have a fatal error 
    - E.g. this error can occur if we initialise a repo with readme and gitignore and other things that locally I dont have
- To update local version, just need to do a pull request 
    - git pull origin main 
- Then can proceed with the push 
    - git push -u origin main 
- Note that this error also occurs if the branch I am trying to push up is not as updated at the remote branch 
    - But if branch dosent exist yet, then can push without any issues 

## Working with Multiple Git Repositories in VS Code (Each linked to a different Github Repo)
- Sometimes you're working on multiple projects (multi-tasking) - each with its own local folder and Github repo. Here's how to manage them efficiently in VSCode. 
    - Opening Multiple Repos (two ways)
        1. Open in Separate VS Code Windows to keep each repo fully isolated 
            1. Open your first project (e.g. Code-References)
            2. Press Cmd + Shift + N (Mac) or Ctrl + Shift + N (Windows) to open a new VS Code window 
            3. In the new window, open another folder (e.g. Cheatsheets)
            In this case, each window has its own file tree, terminal, and Git context 
                - Each terminal window is independent, so can safely push, commit, and even activate diff venv w/o affecting the other 
        2. Use a multi-root workspace: ideal if want everything in one view 
            1. Open first folder 
            2. Go to File > Add Folder to Workspace 
            3. Add the second project folder 
            4. Save the workspace as a .code-workspace file (optional)
            Note: Git actions are still scoped per folder, so make sure in the correct terminal tab or file tree context when committing or pushing 
- Other tips 
    - Be careful with terminal context: even if open a new folder in VS Code, your terminal might still be operating in the previous folder's context. E.g. 
        - Opened Cheatsheets in VS Code 
        - But terminal still in Code-References
        - So running git add git.md will fail since that file lives in Cheatsheets and not Code-References 
        - Hence, always run 
            - pwd: Confirm current working directory 
            - ls: show files in folder 
            - cd ~/path/to/the/folder-you-want 
        -Then can 
            - git checkout -b new_branch 
            - git add file_name 
            - git commit -m "Your message" 
            - git pull -u origin new_branch


## Other notes 
- Sometimes when pulling from remote, will open Vim — the default command-line text editor Git uses for things like merge commits. To exit: 
    - Press Esc (exit any typing mode)
    - Type :wq (write and quit — the colon is important)
    - Press Enter
- If push to main, a pull request will not be triggered as you are not creating another branch
- Github may not automatically show the pull request (PR) banner if you reuse a branch name that was previously used to commit changes, even if that branch has been deleted locally and on Github remotely. To ensure a smoother PR experience, either 
    - Use new, unique branch name for each round of work (i.e. no repeats)
    - Reus the same branch name, but manually create the pull request, either by 
        1. Clicking on the link that appears in the terminal after pushing (i.e. run "git push -u origin branch_name")
            - URL: something like https://github.com/yourname/repo/pull/new/branch_name
        2. Going onto Github manually and 
            - Navigate to repo 
            - Go to Pull Requests tab -> New Pull Request 
            - Set 
                - Base: main 
                - Compare: branch_name 
            - Click Create pull request 
