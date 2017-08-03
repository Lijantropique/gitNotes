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
echo "# gitNotes" >> README.md
git init
git add .
git commit -m "Initial commit"
git remote add origin https://github.com/lijantropique/*newRepoName*.git
git push -u origin master
git status

## Clone existing repository
git clone *repoAddress* [*repoNewName* && cd $_ ]
example:
git clone https://github.com/lijantropique/shablona.git NewProject
(if optional newRepoName is not provided, cd to folder in subsequent step)
git status

## Review Repo's history
git log [--oneline]   -> short description of changes
git log [--stat]      -> additional info of changes (files that has been changed and # lines)
git log [-p]          -> detailed explanation of changes
git log [--stat] [-p] -> both additional info + detailed explanation
git log [-p] [-w]     -> detailed explanation of changes ignoring whitespaces changes

git log [*flags*] *SHA*   ->shows history up to that message
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
Note:  "Initial commit" is de facto comment for the very first commit.
The commit message should be <= 50 lines and tell what the commit does, not how or why. Always use imperative, capital the subject and if you need to use
and probably it would be better to have two commits.
check: https://chris.beams.io/posts/git-commit/

## Difference
git diff        ->changes made but not **staged/committed** yet


## Ignore files
if the file hasn't been tracked yet ...
echo *fileToIgnore* > .gitignore (not considered for staging with ".")
if the file has been tracked:

git rm --cached *fileToIgnore* [-f]
echo *fileToIgnore* > .gitignore

Note: if the file has been modified since the last commit,
it will complain so either the rm needs "-f", to force the removal or commit the changes and then, stop tracking

.gitignore understands globbing: dir1/\*.jpg, would ignore all the jpg files in dir1
check: https://git-scm.com/docs/gitignore


TODO: tagging, branching and merge
TODO: amend, revert, reset
