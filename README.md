# BasisVR Demo Render Pipeline
Fork of Unity's Universal and Core Renderpipeline packages for BasisVR's demo project.

# Syncing with Unity's Graphics Repo
To pull down changes from unity's graphics repo use `rebase` rather than `merge`. Merging will make it impossible to update unity versions as each version has its own staging branch which diverges from main and later staging branches significantly.

To rebase, add the original Graphics repo as the remote `upstream` if you have not already done so:
```
git remote add upstream https://github.com/Unity-Technologies/Graphics.git
```
Fetch the latest changes for the staging branch for the current unity version:
```
git fetch upstream XXXX.X/staging
```
If you are rebasing on the same staging branch, run the following:
```
git rebase upstream/XXXX.X/staging
```
Otherwise, if you are rebasing on a different branch you must find the commit hash of the __parent__ of the first commit of this fork. We need this in order to rebase just our commits on the new branch. This changes every time we rebase, but can be found by searching for the commit message with the following command:
```
git log --pretty=format:"%P" --grep='First Commit. Fix renderpipeline forcing depth priming off if MSAA is enabled. Fix UpdateInstanceOccluders calculating rendertarget resolution incorrectly.'
```
With the commit hash of the __parent__ of the first commit, run the following. This will only rebase the commits after our first commit:
```
git rebase --onto upstream/XXXX.X/staging FULL_COMMIT_HASH_OF_PARENT
```
You will probably run into merge conflicts. Resolve those, and continue until it completes. Finally, either push the new history to a new branch or use `git push --force` to overwrite the history of the old branch. If you want to overwrite, make sure you have a backup of the branch first!
