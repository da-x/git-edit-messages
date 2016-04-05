#!/usr/bin/env python

import os
import sys
import tempfile
import re
import cPickle

EDITOR="EDITOR.TEMP"

class TempFile(object):
    filename = None

    class Info(dict):
        def __init__(self):
            self.nr_commits = 0
            self.cur_commit = 0
            self.remove_numbers = False

    @classmethod
    def write(selfclass, v):
        f = open(selfclass.tempfile, "w")
        f.write(v)
        f.close()

    @classmethod
    def read(selfclass):
        return open(selfclass.tempfile).read()

    @staticmethod
    def load():
        info = TempFile.Info()
        vars(info).update(cPickle.loads(TempFile.read()))
        return info

    @staticmethod
    def save(info):
        TempFile.write(cPickle.dumps(vars(info)))

def editor_rebase_todo(filename):
    output = []
    f = open(filename)
    for line in f:
        if line.startswith('pick'):
            line = 'reword ' + line[5:]
            print line,
            output.append(line)
    f.close()
    f = open(filename, "w")
    f.write(''.join(output))
    f.close()
    info = TempFile.load()
    info.cur_commit = 1
    info.nr_commits = len(output)
    TempFile.save(info)

def editor_commit_editmsg(filename):
    info = TempFile.load()
    f = open(filename)
    output = []
    for line in f:
        if not output:
            m = re.match(r"\[part ([0-9]+)/([0-9]+)\] (.*)", line)
            if m:
                line = m.groups(0)[2] + "\n"
            if not info.remove_numbers:
                line = "[part %d/%d] %s" % (info.cur_commit,
                                            info.nr_commits, line)
        output.append(line)
    f.close()
    info.cur_commit += 1
    f = open(filename, "w")
    f.write(''.join(output))
    f.close()
    TempFile.save(info)

def main():
    if len(sys.argv) >= 4 and sys.argv[1] == EDITOR:
        TempFile.tempfile = sys.argv[2]
        filename = sys.argv[3]
        if filename.endswith('-todo'):
            editor_rebase_todo(filename)
        if filename.endswith('/COMMIT_EDITMSG'):
            editor_commit_editmsg(filename)
        return

    args = sys.argv[1:]
    info = TempFile.Info()
    if '--remove-numbers' in args:
        args.remove('--remove-numbers')
        info.remove_numbers = True
    TempFile.tempfile = tempfile.mktemp("git-rebase-numberer")
    TempFile.save(info)
    try:
        os.putenv("GIT_EDITOR", sys.argv[0] + " " + EDITOR + " " + TempFile.tempfile)
        cmd = ["git", "rebase", "-i"] + args
        os.spawnvp(os.P_WAIT, "git", cmd)
    finally:
        os.unlink(TempFile.tempfile)

if __name__ == "__main__":
    main()
