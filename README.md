# Workflow examples

## Clone (or update) your repository
You need a clone of the repository on your computer.
```
git clone https://github.com/mozilla-l10n/appstores
```
SVN equivalent: `svn checkout https://path/to/svn/repo/locale/code`

Unlike SVN, you will *clone the entire repository will all locales*, not simply your locale’s folder. Unfortunately this can't be avoided.

If you already cloned the repository, always make sure to update your local clone. From within the clone’s folder:
```
git pull
```
SVN equivalent: `svn update`

## Update your files
Nothing has changed from SVN. Update the files you need to update using your editor of choice.

## Commit your updates
First of all, check which file changed in your local clone:
```
git status
```
SVN equivalent: `svn status`

You'll get a list of all changed files:

```
On branch master
Your branch is up-to-date with 'origin/master'.
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

    modified:   it/description_beta_page.lang

no changes added to commit (use "git add" and/or "git commit -a")
```

Now add the changed file, commit and push

```
git add it/description_beta_page.lang
git commit -m "Updated Beta description for Italian"
git push
```

SVN equivalent (you don't need add a file or push)
```
svn commit -m "fr: fixed a typo in main.lang"
```

## Deal with merge conflicts
As explained above, you'll clone the entire repository with all locales, so you might get this kind of error when trying to push your changes to the repository. It simply means another localizer has committed changes to master after your last `git pull`(again, always make sure to update your clone, especially before starting to work).
```
git push
To https://github.com/mozilla-l10n/appstores
 ! [rejected]        master -> master (fetch first)
error: failed to push some refs to 'https://github.com/mozilla-l10n/appstores'
hint: Updates were rejected because the remote contains work that you do
hint: not have locally. This is usually caused by another repository pushing
hint: to the same ref. You may want to first integrate the remote changes
hint: (e.g., 'git pull ...') before pushing again.
hint: See the 'Note about fast-forwards' in 'git push --help' for details.
```

To fix this you need to pull the last commits, merge and push
```
git pull
(git will try automatically to merge, confirm the commit message that will appear in your editor)
git push
```

For more details about Git and GitHub, especially if you have conflicts that can't be merged automatically, see also [this guide](http://flod.org/github/).

# FAQ
## How can I get only my locale in the repository and not all locales
Unlike svn, git does not allow cloning a subdirectory only from a repository, so you have to download the whole repository and you get to see the folders for all locales.

The possibility to download a subdirectory only is called "sparse checkout". Relatively recent (>1.7) version of git allow performing a sparse checkout with a few command lines. Here is how to do it:
```
 git clone https://github.com/mozilla-l10n/appstores --no-checkout
 cd appstores
 git config core.sparsecheckout true
 echo "fr/*" > .git/info/sparse-checkout
 git checkout --
 ```
If you look at your local clone, it now contains only a 'fr' folder, you don't see all the locales anymore.

The difference with svn is that in git is actually hides the other folders, while in svn you would have downloaded only the folder you wanted.
