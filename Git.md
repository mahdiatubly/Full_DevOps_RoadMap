# Git

## Some tricky commands

- To see the changes in a file:

        $git diff [file name]

- To add just a part of the changes in a file to the staging are:

        $git add -p [file name]

- To discover the branches that the current branch is istantiated from:

        $git log --graph --decorate

- To add a remote repo to upload the git repo on your local computer to github:

        $git remote add origin https://github.com/mahdiatubly/test.git
        $git push origin main

- To create a new branch:

        $git branch [the branch name]
        $git branch [the branch name] [the commit that you want to start the branch with]

- To change the name of a branch:

        # Switch to the branch
        $git branch -m [new name]
        # With Switching to the branch
        $git branch -m [old name] [new name]

- To get a branch from a remote repo:

        $git branch --track [the neme of the new branch] origin/[branch name]
        # Or
        $git checkout --track origin/[branch name (will be tacken by the new local one)]

- To get the difference between the local and the remote repo:

        $git branch -v

- To see the difference between two brances in term of commits:

        $git log [first branch] [the other branch]

- To delete a branch in the repo:

        $git branch --delete [branch name]

- To delete a branch in the remote repo:

        $git push --delete [branch name]

- To merge a branch you have to be in the main branch, then:

        #git merge [the name of the branch that you want to merge]
        $git merge authentication

- To get the updates in the remote repo:

        # May be the main branch called master in some github accounts.
        $git pull origin main

- To remove a commit:

        #With --hard will remove the commit and all the changes in it.
        $git reset --hard HEAD~1

        #With --soft will leave the changes in the staging area.
        $git reset --soft HEAD~1

        #Also you can use revert which will create a new commit that shows that the commits change has deleted or reverted.
        $git revert 0953213565.....[the required commit SHA]

- To transfer changes from main branch to yours without lefting a merging commit you can use rebasing. (However, notice that rebasing will change the hashs of the commites where it creates a new copies of the commits not just transfering them. FOR THAT IT USED JUST WITH LOCAL REPO'S):

        #The current branch will be on top of main branch
        $git rebase main
        #To change a commit message with rebase command:
        $git rebase -i Head~[# number going forward from the head]
        #The previous command will open the editor, add `reward` before the commit that you want to update, save and close and that will open a new window for you to update the commit message. Add squash before the commit to combine it with the one above it.

- To get a specific in a branch, first go to the branch that you want to transfer the commmit to it, then run the following command:

        $git cherry-pick 0953213565.....[the required commit SHA]

- In the case that you want to work with a previous commit while you have new change that is not ready to be committed, then you can save them in the stash:

        #To add changes to the stashing area:
        $git stash

        #To retrieve the changes
        $git stash pop stash@{0}

        #To list all the changes in the stash
        $git stash list

        #To show the changes in the stash
        $git stash show stash@{0}

- To see all the log of the reseting actions:

        $git reflog

        To return back to a reseted commit from ref log
        $git reset --hard 09635135...[commit SHA]

- To add submodules to your repo:

        $git submodule add [link of the modules repo]

- To download submodules while cloning a repo contains them:

        $git clone --recurse-submodules [repo url]

- To serch in git log using date:

        $git log --after="2022-7-1" --before="2022-12-8"

- To serch in git log using a word in the commit message:

        $git log --grep="Love" --author="MA"

- To serch in git log using a word in the commit message:

        $git log --grep="Love" --author="MA"

- To serch in git log using a file changed in the commit:

        $git log --[filename]

- If you want to return back to the version of the last commit:

        # You can add -p flage if you want to remove just a few changes.
        $git restore [file name]

        #To restore a specific commit for just a specific file
        $git --source [commit SHA] [file name]

- To update the commit message of the last commit:

        $git commit --ammend -m "The new commit message"

- To fix an old commit:

        $git commit --fixup [commit SHA]
        $git rebase -i HEAD~[old commit SHA] --autosquash
