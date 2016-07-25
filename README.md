## ASteCA new release

Steps detailing how to publish a new release after the development on the
`develop` branch is completed. We follow the [git-flow][1] branching
model.

### 1. Update .first_run file

1. If this file needs to be modified, remove it from `--skip-worktree` so
that it will be tracked:
  ````
  git update-index --no-skip-worktree packages/.first_run
  ````

1. Make any necessary changes to this file.

1. Push changes (if any):
  ````
  # acp --> alias for 'add + commit + push'
  git acp 'update first_run file'
  ````

### 2. Create `release` branch

1. Create a new `release` branch from `develop`. Set `<version>` to the
version number that will be released.

  ````
  # co --> alias for checkout
  git co -b release-<version> develop
  ````

### 3. Prepare new release

1. Add the changes made from latest release to `CHANGELOG.md` file.
**Remember to link the issues with markdown.**

1. Add & commit above changes to `CHAGELOG.md` file:
  ````
  # ac --> alias for 'add + commit'
  git ac 'update changelog'
  ````

1. Add version number `<version>` to `_version.py` file:
  > __version__ = "vx.x.x"

1. Add & commit above edit. This is the *last commit* in the branch before the
final release, **check carefully.**
  ````
  git ac 'Bumped version number to <version>'
  ````

1. If some last minute change is necessary, **do it now**.

### 4. Finish `release` branch

1. Merge `release-<version>` branch into `master` branch (there should be
no conflicts):
  ````
  git co master
  git merge --no-ff release-<version>
  ````

1. Push merged `master` branch????
  ````
  git push
  ````

1. Tag this new release `vx.x.x` and push new tag:
  ````
  git tag -a vx.x.x
  git push --tags
  ````

### 5. Remove `release` branch

1. Merge `release` branch back into `develop`. If this leads to a merge
conflict, fix it and commit.
  ````
  git co develop
  git merge --no-ff release-<version>
  ````

1. Remove `release` branch:
  ````
  git branch -d release-<version>
  ````

1. Push changes in `develop`???
  ````
  git push
  ````

1. Ignore `.first_run` file again:
  ````
  git update-index --skip-worktree packages/.first_run
  ````

### 6. Release in Github

1. Link draft release with new tag in Github and publish. **New version is
now fully released**.

   https://github.com/asteca/asteca/releases


### 7. Update site and docs

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


________________________________________________________________________________
[1]: http://nvie.com/posts/a-successful-git-branching-model/
