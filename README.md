Step by step instructions to publish a new ASteCA release.

# New release steps

### Publish new release after development on active branch is completed

1. Add the changes made in latest release to `CHANGELOG.md` file. **Remember to
link the issues with markdown.**

1. Push above changes to `CHAGELOG.md` file:
  ````
  git acp 'update changelog'   # acp --> add + commit + push
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

1. Merge new `vxxx` branch into `master` branch:
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

1. Link draft release with new tag in Github and publish.

   https://github.com/asteca/asteca/releases

1. Add new published version to `index.html` file in:

   http://asteca.github.io

1. Push above changes made to the site. **New version is now fully released**.
  ````
  git acp 'release vx.x.x'
  ````

### Create new active branch

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
