###notes based on youtube video : https://youtu.be/apGV9Kg7ics?si=dUwdAZqv6mHF13Ez

1. `git init`: intialize git in a project folder (this will be our git repo)
2. `git status`: to see any untracked/tracked changes(not committed) in repo
3. `git add`: add untracked changes to repo
   <git add .>: adds all untracked files to repo stage
   <git add file_name>: add only the named file to repo
4. `git commit` : to commit the changes to repo, anything that was added using git add, gets commited
    <git commit -m "file added"> : -m to add the commit msg
5. `git restore`: to remove a modified file added acidently from git repo
   <git restore --staged file_name>: remove file from repo stage
6. `git log`: to view commit history of current repo
7. `git reset`: resets the repo to given commit, all the commits above it in stack gets deleted. And the deleted files go back in unstaged section
   <git reset commit_id>
8. `git stash`: safekeeps the modified files without committing them , you first in to add them to stage before stashing
   <git add filename>
   <git stash>: adds the add files to stash
   <git stash clear>: clears the files from stash, they are gone for good, cannot get them back
   <git stash pop>: pop the stash back to original stage

9. `add github repo to git`:
    <git remote add origin https://repo_url>
    <git remote -v>: displays all the origin URL in your repo

10. `add files to github`:
    <git push origin master>: git repo pushed to master branch in github

11. `create new branch`: 
    <git branch branch_name>: when new branch is created.

12. `to navigate b/w branches`: to move from one branch to other. The command makes HEAD points to given branch name, which means new commit will be added to this branch
    <git checkout branch_name>

13. `to merge branch`:
    <git merge branch_name>

14. `to update existing git repo `: for repos from other accounts, you first need to fork it into your account. Then you can clone it in repo and start making changes
    <git clone forked_repo_url>

The repo from which you have forked is called upstream  and you can add it using 
<git remote add upstream original_repo_link>

15. `Pull request`: 
- pull is used to keep in sync with main branch and upstream
- to merge the code changes in cloned repo branch to the main branch, we sent a pull request. The owner of the main branch can then view the request and approve to merge it
- one branch can allow only one pull request, that's why we should never push/pull to the main branch, instead create different branches for different features and send pull request to those feature branches, then those feature branches can later be merger to main branch 
    <git pull origin branch_name>

- to remove commit from github, you need to stash the commit and then force push it again 
  <git push origin Yashi -f>

- to reset your branch with upstrem branch 
  <git pull upstream main> : the below steps mentioned in fetch is done in this one single command

16.  `git fetch`: to fetch changes in main branch to your local branch 
    <git checkout main> : first point to main branch
    <git fetch --all --prune>: fetches all the brancges....prune fetches related branches as well
    <git reset --hard upstream/main> : set my main branch to main branch of upstrem
    <git push origin main>

17. `merge commits/squashing commits`: to merge multiple commits in a single commit
    <git rebase -i commit_id>: open a interactive terminal
    - can change pick to squash (s), by this the squashed commit will be added in the one single pick commit. The squashed commits are merged in the pick commit just above them. 
    - to get out ot interactive terminal press esc + :x
  
18. `merge conflicts`: when changes are made on same line by different people
- conflicts need to be resolved manually, whenever there is overriding