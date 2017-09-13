# Git - Best practices

## General
Always use rebase instead of merge.

### Master branch
It's been prohibited to perform commits on the master branch.

### Feature branches
Every story in Jira should have a corresponding feature branch with naming convention 'features/pres-xx'. Keep in mind that all branches should be in lower case as this might result in issues for OSX users.
When a (part of a) feature needs to be pushed to master branch then a pull request needs to be made.

### Local branch
You should always develop on local branches that are branched from a feature branch. Before pushing your changes to the feature branch you should:
* Squash your commits in logical blocks. Keep as less commits as possible but also keep the possibility for cherry picking.
    {% highlight bash %}git rebase -i HEAD~3{% endhighlight %}
* Follow the same procedure for a correct rebase.

### Commits
Every commit should start with the correct Jira ticket. This way the commit can be matched with the ticket in Jira.

## Correct rebase & Pull request for master branch
Example below is to merge your changes on feature branch 'features/PRES-XX' to master. However same procedure should be followed if you want to merge local branch to a feature branch.

* You should have no pending changes in the git folder. In case you have, they should be committed or stashed before you continue.
* Go to master branch
    {% highlight bash %}git checkout master{% endhighlight %}
* Make sure you have the latest version of the master branch.
    {% highlight bash %}git pull --rebase origin master{% endhighlight %}
* Go to the feature branch
    {% highlight bash %}git checkout features/pres-xx{% endhighlight %}
* Make sure you have the latest code on your feature branch
    {% highlight bash %}git pull --rebase origin features/pres-xx{% endhighlight %}
* Optional: Squach your commits before going further, this can be done to cleanup the git history before it's added to the master branch. Best practice however is do this on your local branch, you are not supposed to squash commits from other developers. The number behind HEAD~ is used to determine how many commits will be shown in the interactive rebase. After the sqush you will need to force push your commits, before you do this you must be sure that noone else is committing on the feature branch.
    {% highlight bash %}
    git rebase -i HEAD~3
    //Make sure no new commits have been done on the feature branch
    git push -f origin features/pres-xx
    {% endhighlight %}
* Rebase the feature branch with master
    {% highlight bash %}git rebase master{% endhighlight %}
* In case there are conflicts due to the rebase they must be resolved. Once they are solved you can continue your rebase
    {% highlight bash %}
    git add .
    git rebase --continue
    {% endhighlight %}
* Make sure that during this process there hasn't been a commit on master branch. In case there was you will need to redo the steps.
* You are ready to push the changes to your feature branch
    {% highlight bash %}git push -f origin features/pres-xx{% endhighlight %}

After this commit a pull request can be created in github.
In case you don't need to create a pull request (merging between branches) then you can merge them:
    {% highlight bash %}git merge feature/gulp-package --ff-only {% endhighlight %}
