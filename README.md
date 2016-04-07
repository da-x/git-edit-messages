## About

   "_GSR, GMR, and the work is done!_"

`git-mechanized-rebase`, or `GMR`, is a utility on top of Git for the automation
of various `rebase --interactive` procedures, using an elaborated command line
syntax, instead of launching an editor.

### Examples

Swap the two topmost commits:

```
$ gmr HEAD~2 :swap ^0 ^1

Successfully rebased and updated refs/heads/master.
```

Be more instructive and look at the rebase script before and after it is modified:

```
$ gmr HEAD~2 :show :swap ^0 ^1 :show

@0     ^1     pick 3310fe6d5b99 First
@1     ^0     pick a9de41e65cbb Second

@0     ^1     pick a9de41e65cbb Second
@1     ^0     pick 3310fe6d5b99 First

Successfully rebased and updated refs/heads/master.
```

### Syntax

```
gmr <rebase-params> <commands>

where:

    <rebase-params>: The standard parameters for `rebase`,
                     for example, `origin/master` or `--onto some-branch HEAD~5.
                     See the man page of `git rebase`.

    <commands>: Operations to do on the rebase script and on commits.
                The order of the commands is important, as they operate
                on the `rebase --interactive` script one after another.

    First, we need to know of to specify commits. Let's call our commit or
    commit group specifiers expression '<+>', and it is an extension of the
    standard way of specifing revisions in (see `man gitrevisions`). We can
    make use of the fact that that we are working with a linear list rather
    than a tree to make it shorter, e.g. ~4-~2 instead of HEAD~4..HEAD~2.

    <+> is thus:

      Single commits:

        ^<integer>          - a 0-based index in the `rebase --interactive` script,
                              where the buttom in the list is index 0, the last
                              from the  buttom it 1, and so on.
        @<integer>          - a 0-based index in the `rebase --interactive` script,
                              where the top of the list is index 0, the next one
                              down is 1 and so on.
[TODO]  <rev>               - a Git revision of a commit, which must exist in the
                              `rebase --interactive` script, (see `man gitrevisions`)

      Ranges of commits:

[TODO]  [@|^]<integer>-[@|^]<integer>
                              - a 0-based groups of indicts in the rebase interactive
                                ToDo list, inclusive only on its beginning.
[TODO]  [@|^]<integer>-       - a 0-based groups of indicts in `rebase --interactive`
                                script, taking all the indicts until the end of this
                                list.
[TODO]  -[@|^]<integer>       - a 0-based groups of indicts in `rebase --interactive`
                                script, taking all the indicts until from the
                                beginning until the given integer, non inclusive.

[TODO]  -                   - all indicts.
[TODO]  <rev>..<rev>        - e.g. HEAD~2..HEAD, a Git range, non inclusive
                              in its begining (see `man gitrevisions`).

    Note that Git revisions are resolved before GMR does anything!

    Commands for introspection:

        :show                - print the rebase script at this stage

    Commands for editing the 'rebase interactive' commit list:

[TODO]  :fixup <+>           - mark commit(s) for fixup
[TODO]  :squash <+>          - mark commit(s) for squash
[TODO]  :reword <+>          - mark commit(s) for reword (launching your editor)
[TODO]  :del <+>             - delete the commit(s) from the list
        :swap <+> <+>        - swap the two given commit groups, preserving
                               their internal order (they must not overlap).
[TODO]  :top <+>             - take the given commit(s) to the top of the list
                               (to be picked first)
[TODO]  :bottom <+>          - take the given commit(s) to the bottom of the list
                               (to be picked last)
[TODO]  :movepre <+> <+>     - move the given commit(s) in first paramter to be
                               before the given commits in the second paramter.
[TODO]  :movepost <+> <+>    - move the given commit(s) in first paramter to be
                               after the given commit(s) in the second paramter.
[TODO]  :insertpre <+> <+>   - [TBD]
[TODO]  :insertpost <+> <+>  - [TBD]

    Commands for editing the commit themselves, using GSR:

[TODO]  :gsrc <+> <gsrfunc>  - perform GSR on the commit messages of the
                               specified commits.
[TODO]  :gsrd <+> <gsrfunc>  - perform GSR on the commit diffs of the
                               specified commits.
[TODO]  :gsr <+> <gsrfunc>   - perform GSR on the both the commits diffs and
                               the commit messages.

    Commands for issue-tracking labeling:

[TODO]  :labelpre  <+> <str> - add a bracket enclose label to the beginning of
                               the given commit(s)' message subject lines.
[TODO]  :labelpost <+> <str> - add a bracket enclose label to the end of
                               the given commit(s)' message subject lines.
[TODO]  :labelrm   <+> <str> - detect the given bracket enclosed label and
                               remove it from given commit(s)' message subject
                               lines.

```
