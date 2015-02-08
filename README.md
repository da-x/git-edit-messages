About
-----

*git-rebase-numberer* is a small utility on top of plain `git` for
adding numbering to commit messages. It wraps around 'git rebase -i',
replacing the GIT_EDITOR and performing the automatic amending of
commits in the branch.

For example, suppose that you have the following history in the branch:

    work: first commit commit
    work: second commit
    work: some fix to the first

The first line will be modified as such:

    [part 1/3] work: first commit message
    [part 2/3] work: second commit message
    [part 3/3] work: some fix to the first

You can provide --remove-numbers in order to do the inverse on
the commit messages, which is to remove the numbering from the
commits.

Syntax:

    git-rebase-numberer.py [addition args to 'git rebase -i'] (--remove-numbers)
