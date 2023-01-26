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

- To transfer changes from main branch to yours without lefting a merging commit you can use rebasing. (However, notice that rebasing will change the hashs of the commites where it creates a new copies of the commits not just transfering them):

        #The current branch will be on top of main branch
        $git rebase main

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
