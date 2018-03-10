# ASteCA branching model

Steps detailing how to publish a new release after the development on the
`develop` branch is completed. We follow the [git-flow][1] branching
model (also described [here][2]).


## Working on a new feature/issue

### 1. Create branch from `develop` to work on a given feature
  ````
  # Checkout from develop branch; co --> alias for checkout
  git co -b <branch>
  # Push and track feature branch
  git push --set-upstream origin <branch>
  ````

### 2. After feature is finished, merge back into `develop`
  ````
  # Switch to develop
  git co develop
  # Merge into develop
  git merge --no-ff <branch>
  # Push changes
  git push origin develop
  # Delete remote branch (optional)
  git push origin --delete <branch>
  # Delete local branch (optional)
  git branch -d <branch>
  ````

## Publishing a new release

### 0. Check that everything works

Download the `develop` version into a new `conda` encironment, and run to see that everything works as expected.

### 1. Create `release` branch, update changelog and version

1. Create `release` branch locally
    ````
    git co -b release-<version> develop
    ````

   **Optional** Update `.first_run` file if it needs to be modified. Remove it
   from `--skip-worktree` so that it will be tracked. Make any necessary changes
   and commit.
    ````
    # Track again
    git update-index --no-skip-worktree packages/.first_run
    # Make changes and commit. ac  --> alias for 'add + commit'
    git ac 'update first_run file'
    ````
1. Add & commit the changes made since latest release to `CHANGELOG.md` file.
   **Remember to link the issues with markdown.**
    ````
    git ac 'update changelog'
    ````
1. Add & commit version number `<version>` to `_version.py` file. This is the
   *last commit* in the branch before the final release, **check carefully.**
    ````
    git ac 'Bumped version number to <version>'
    ````
1. If some last minute change is necessary, **do it now**.


### 2. Finish `release` branch

1. Merge `release` branch into `master` and push.
    ````
    git co master
    git merge --no-ff release-<version>
    git push
    ````
1. Tag this new release `vx.x.x` and push new tag.
    ````
    git tag vx.x.x
    git push --tags
    ````
1. Merge `release` branch into `develop`, and delete.
    ````
    git co develop
    git merge --no-ff release-<version>
    git push
    git branch -d release-<version>
    ````
1. Add `dev` to the version number in `_version.py` file. This is the
   *first commit* in the new  `develop` branch.
    ````
    git acp 'mark branch as develop'
    ````

   **Optional** Ignore `.first_run` file again.
    ````
    git update-index --skip-worktree packages/.first_run
    ````

### 3. Release in Github

Link draft release with new tag in Github and publish
(https://github.com/asteca/asteca/releases).

**New version is now fully released**.


### 4. Update site and docs

1. Add new published version to `index.html` file in:

   http://asteca.github.io

1. Push above changes made to the site.
    ````
    git acp 'release vx.x.x'
    ````

1. Add new version to `conf.py` file in docs.

1. Make any necessary changes/additions to the docs.

1. Push above changes made to the docs.
    ````
    git acp 'release vx.x.x'
    ````


[1]: http://nvie.com/posts/a-successful-git-branching-model/
[2]: https://www.atlassian.com/git/tutorials/comparing-workflows/gitflow-workflow
