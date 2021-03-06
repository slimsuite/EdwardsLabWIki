# Creating the EdwardsLabWiki github
This page gives an overview of the creation process for the `EdwardsLabWiki` using [GitHub][1] and associated git tools and commands.

## Step 1: Create GitHub repository
See instructions on GitHub if unsure. I created a [README.md][2] file to have a git repo for cloning in this case. The alternative is to create an empty repo and push into it. I'll be trying this below with the wiki!

## Step 2: Clone repository
```
git clone https://github.com/slimsuite/EdwardsLabWiki.git
```
As I am intending to push the same repo into the wiki, I copied the [README.md][2] file as the wiki's initial [HOME.md][3] page:
```
cp README.md Home.md
```
I then made the initial version of this [Creation.md][4] file. Within this, I am trying some "local" markdown file links, e.g. `[Home.md](./Home.md)`. I am not sure whether this works!

## Step 3: Create the wiki repository
I use the **GitHub Desktop App** for generating my commits and **GitUp** for visualising the branching structure - **WARNING:** I intend to make use of branching for this repo!
**NOTE:** I tried to do this by creating a new branch and pushing:
```
git branch wiki
git checkout wiki
git remote add githubwiki https://github.com/slimsuite/EdwardsLabWiki.wiki.git
git push -u githubwiki master
```
This did not work!:
```
remote: Not Found
fatal: repository 'https://github.com/slimsuite/EdwardsLabWiki.wiki.git/' not found
```
» Needed to create the wiki repo first on github. This was made with a `Temp.md` file to avoid conflicts when trying to merge repos. Here we go...

## Step 4: Merge repos
An inital attempt to push to the repo was rejected because it had files needing to be merged. I think the following can be simplified, but here is what I did...

**4.1. Merge in the wiki**

```
git remote add githubwiki https://github.com/slimsuite/EdwardsLabWiki.wiki.git    
git pull githubwiki master
```
This generate a merge in the `wiki` branch but it could still not be pushed to the repo.

**4.2. Make a new merger branch**

```
git checkout -b merger --track githubwiki/master
git merge wiki
```
This was then pushed to the Wiki repo.

**4.3. Merge and push main repo.**

The `wiki` branch seemed surplus to requirements. Will try removing it and renaming merger to wiki. Then move master to point at the same commit:

```
git checkout master
git merge merger
git push
git branch -D wiki
git branch -m merger wiki
```

`master` was also pushed to `origin`. Both repos are now synced up, with `master` pointing to `origin` and `wiki` pointing to `githubwiki`:

![GitUp branch view](screenshots/GitUp-merged.png)

## Step 5. Keeping the repos synced
To maintain the sync, it is just a case of merging and pushing the branches at appropriate times. e.g. after an commit to branch `master`:

```
git push
git checkout wiki
git merge master
git push githubwiki HEAD:master
```

**Note:** the last push is a bit more complex than perhaps necessary as the remote branch has a different name to the local one. The wiki always displays the `master` branch, I think, whereas the main github page will have both branches. It probably therefore makes sense to link the `master` branch to the wiki and another branch to GitHub. Or just have the one master branch pushing to both? Going to try that now on `master` after a commit:

```
git push
git push githubwiki
```


---
<small>Last Edit: 20 Jul 2018</small>


[1]: https://github.com/
[2]: https://github.com/slimsuite/EdwardsLabWiki/README.md
[3]: ./Home
[4]: https://github.com/slimsuite/EdwardsLabWiki/Creation.md