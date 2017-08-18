# Git Basic Commands

## Global configuration
git config --global user.name  "*username*"
git config --global user.email "*email@address*"
git config --global color.ui auto
git config --global merge.conflictstyle diff3
git config --global core.editor "atom --wait"
git config --list

## Start a new repository
mkdir -p *newRepoName* && cd $_
echo "# *newRepoName*" >> README.md
git init
git add .
git commit -m "Initial commit"
git remote add origin https://github.com/lijantropique/*newRepoName*.git
git push -u origin master
git status

## Clone existing repository
git clone *repoAddress* [*repoNewName* && cd $_ ]
example:
git clone https://github.com/lijantropique/shablona.git newRepoName && cd $_
(if optional newRepoName is not provided, cd to folder in subsequent step)
git status

## Review Repo's history
git log [--oneline]   -> short description of changes
git log [--stat]      -> additional info of changes (files that has been changed and # lines)
git log [-p]          -> detailed explanation of changes
git log [--stat] [-p] -> both additional info + detailed explanation
git log [-p] [-w]     -> detailed explanation of changes ignoring whitespaces changes

git log [*flags*] *SHA*   -> shows history up to that message
git shows                 -> display the most recent commit
git shows [*flags*] *SHA* -> display only that commit

## Add commit
after working/saving a file...
git add *fileName*    -> stages a file
git add .             -> stages all files in current directory
git rm --cached *filename* -> unstage a file
git reset              -> unstage all the files NOTE: --mixed provided as default flag

git commit -m "*commitMessage*"
alternatively:
git commit     -> wait for atom to open for adding message
NOTE:  "Initial commit" is de facto comment for the very first commit.
The commit message should be <= 50 lines and tell what the commit does, not how or why. Always use imperative, capital the subject and if you need to use
and probably it would be better to have two commits.
check: https://chris.beams.io/posts/git-commit/

## Difference
git diff        -> changes made but not **staged/committed** yet


## Ignore files
if the file hasn't been tracked yet ...
echo *fileToIgnore* > .gitignore (not considered for staging with ".")

if the file has been tracked:
git rm --cached *fileToIgnore* [-f]
echo *fileToIgnore* > .gitignore

NOTE: if the file has been modified since the last commit,
it will complain so either the rm needs "-f", to force the removal or commit the changes and then, stop tracking

.gitignore understands globbing:
dir1/\*.jpg, would ignore all the jpg files in dir1
check: https://git-scm.com/docs/gitignore

## Tagging
to add an annotated tag
git tag -a *tagName* -m  "*tagMessage*" [*SHA*]
If the -m option is not included, git will open the editor to provide the tag message.
If the digest is not provided, it would tag the latest commit.
NOTE: since version 2.13, git log includes --decorate. If previous versions it would be required to add this option to see the tags.

The tag information could be obtained using shows
git show *tagName*

to delete a tag:
git tag -d *tagName*

if lightweight tags (kind of temporary or private tags) are required:
git tag -lw *tagName*
NOTE: don't supply -a,-s or -m option.
checck: https://stackoverflow.com/questions/11514075


## Branching
git branch *branchName* [*SHA*]
git checkout *branchName*

to create and change to the branchName
git checkout -b *branchName* [*SHA*]

if ths diggest is not provided, the branch would be based on the current HEAD position. Otherwise, it would be created on the commit specified by the diggest.

git branch   -> all the branches and the active branch

to delete a branch:
git branch -d *branchName*
NOTE: it is not possible to delete the current branch. It is required first to move (checkout) to another branch and then delete.

git normally won't delete branches with unique commits (commits that are not included in any other branch), because those commits would be lost. However, to force deletion:
git branch -D *branchName*

to see all branches at once:
git log --oneline --graph --all


## Merging
first checkout the branch used as basis for the merge
git checkout *branchMergeBasis*
git merge *branchMergeOther*

Git will look back at the history for a common commit between the two branches. Once it has found it, it would combine the lines of code and add the commit on the basis (branchMergeBasis). The other one (branchMergeOther) will remain as it was before.

It the branch to merge (branchMergeOther) is ahead of the base, it would be a fast forward merge. Otherwise it would be a regular merge.

Git will open the code editor to provide a message, but there is a default msg that is just fine in most of the cases.


## Merge Conflicts
Sometimes there are conflicts between the branches (i.e., each branch has a different status for the same line) and instead of choosing one, Git will flag those issues. NOTE: actually there are commands (-s ours, -s theirs) that can be used to force one branch.

At this point it would be possible to abort the merge
git merge --abort

Or resolve the conflicts.  n the global configuration file, it was declared that we would like to use the diff3 style, which presents conflicts as follows:

.<<<<<<<<<<  HEAD
...       -> everything here is what is in the current branch
||||||||||  merged common ancestors
...       -> everything here is what was before (where the two branches had a common ancestor)
==========  
...       -> everything here is what is in branchMergeOther
.>>>>>>>>>> *branchMergeOther*

**merge conflicts must be resolved by hand on the editor, by removing the
conflict indicators (<<<<< |||| ==== >>>>) and selecting which line to keep**

after the conflict is resolve, save, add & commit (leave default merge commit message).

## Change the last commit
git commit --amend
NOTE: if any file is modified, saved and staged, the amend option would add that file to the commit. Otherwise, it would only change the last commit message.

## Reverting a commit
git revert *SHA*
it would do exactly the opposite of the changes of that specific commit (e.g., if a line was added, it would be deleted), as a new commit.

## Reset a commit
** This command will erase a commit!!! (small backup of 30d with git reflog)**


To reset a commit it is required to use relative references:
^   -> parent
~n  -> nth parent

HEAD^ / HEAD~ / HEAD~1  -> parent commit
HEAD^^ / HEAD~2         -> grandparent commit
HEAD^^^ / HEAD~3        -> great-grandparent commit

with merge, there are two parents
^   -> first parent
^2  -> second parent

there are three flags for reset
* --mixed (default) -> the commit content would be moved to the working directory
* --soft            -> the commit content would be moved to the staging directory
* --hard            -> the commit content would be discarded

git reset [--flag] *relativeRefCommit*


<!-- ====================== -->
TODO: add git remote set-head origin -d
check https://www.kernel.org/pub/software/scm/git/docs/git-remote.html
