# Git

## Some of tricky commands

- To discover the branches that the current branch is istantiated from:

        $git log --graph --decorate

- To add a remote repo to upload the git repo on your local computer to github:

        $git remote add origin https://github.com/mahdiatubly/test.git
        $git push origin main

- To merge a branch you have to be in the main branch, then:

        #git merge [the name of the branch that you want to merge]
        $git merge authentication

- To get the updates in the repo:

        # May be the main branch called master in some github accounts.
        $git pull origin main

- To remove a commit:

        #You have to change the head from the last commit to be deletable.
        $git reset --hard HEAD~1
