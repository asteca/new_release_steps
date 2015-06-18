## ASteCA new release

Steps detailing how to publish a new release after the development on the
active branch is completed.

### 1. Update input parameters files

1. Move *used* `*_input.dat` files into the `def_params/` folder.

1. Move *default* `*_input.dat` files into the top level folder and rename them
removing the initial `d_` from their names.

1. Remove files from `--assume-unchanged` so they will be tracked:
  ````
  git update-index --no-assume-unchanged *_input.dat
  ````

1. Push changes on both these files (if any):
  ````
  git acp 'update input files'   # acp --> add + commit + push
  ````

1. Stop tracking these files again:
  ````
  git update-index --assume-unchanged *_input.dat
  ````

1. Rename adding `d_` to the beginning of both files (`d_*_input.dat`) and move
them into the `def_params/` folder again.

1. Move used `*_input.dat` files back to the top level.

### 2. Create new release

1. Add the changes made in latest release to `CHANGELOG.md` file. **Remember to
link the issues with markdown.**

1. Push above changes to `CHAGELOG.md` file:
  ````
  git acp 'update changelog'
  ````

1. Remove _beta_ from version number in `__init.py__`.
  > __version__ = "vx.x.x-beta" --> __version__ = "vx.x.x"

1. Push above edit. This is the *last commit* in the branch before the final
release, **check carefully.**
  ````
  git acp 'up version'
  ````

1. Rename `active` branch to new `vxxx` version name, delete old `active`
branch and push the new branch upstream:
  ````
  git branch -m active vxxx             # Rename branch locally
  git push origin :active               # Delete the old branch
  git push --set-upstream origin vxxx   # Push the new branch, set local branch
  to track the new remote
  ````

1. Merge new `vxxx` branch into `master` branch (there should be no conflicts):
  ````
  git rebase master
  git co master       # co --> checkout
  git merge vxxx
  ````

1. Push merged `master` branch:
  ````
  git push
  ````

1. Tag this new release `vx.x.x` and push new tag:
  ````
  git tag vx.x.x
  git push --tags
  ````

1. Link draft release with new tag in Github and publish. **New version is
now fully released**.

   https://github.com/asteca/asteca/releases

### 3. Update site and docs

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

### 4. Create new active branch

1. Create new `active` branch locally, branched from `master`:
  ````
  git co -b active
  ````

1. Push and track new `active` branch:
  ````
  git push --all -u
  ````

1. Increase version number and set as beta in `__init.py__` file:
  > __version__ = "vx.x.x" --> __version__ = "v(x.x.x + 1)-beta"

1. Push beta version number (first commit in branch):
  ````
  git acp 'beta version'
  ````
