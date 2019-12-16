# Deep Dive Git

> Git is great until you have to work with others. - Nick Nisi
Youtube Video: Rules to git by.

[Youtube Link](https://www.youtube.com/watch?v=yI0BtEzdGtw)

EVENT: NebraskaJS

SPEAKER: Nick Nisi

## Contents

======

If you run following command in root of git repo

```bash
tree .git/
```

## .git Folder Structure

```bash
.git/
├── branches
├── COMMIT_EDITMSG
├── config
├── description
├── HEAD
├── hooks
│   ├── applypatch-msg.sample
│   ├── commit-msg.sample
│   ├── fsmonitor-watchman.sample
│   ├── post-update.sample
│   ├── pre-applypatch.sample
│   ├── pre-commit.sample
│   ├── prepare-commit-msg.sample
│   ├── pre-push.sample
│   ├── pre-rebase.sample
│   ├── pre-receive.sample
│   └── update.sample
├── index
├── info
│   └── exclude
├── logs
│   ├── HEAD
│   └── refs
│       └── heads
│           ├── jassi
│           └── master
├── objects
│   ├── 0b
│   │   └── 2963a7855ab412c8838cc1883802f6588c4d2a
│   ├── 2d
│   │   └── 15f810f1b14e0c41560bd19aea42db704bcc8c
│   ├── 6f
│   │   └── 0d725631453bc11db9e31017a68488dd7cd99e
│   ├── bf
│   │   └── 6b8179632b5711ffa21a9869186fae9199d99c
│   ├── e6
│   │   └── 9de29bb2d1d6434b8b29ae775ad8c2e48c5391
│   ├── f0
│   │   └── ad0dec66b594ef1f8bdd71081bdad977b828d4
│   ├── info
│   └── pack
└── refs
    ├── heads
    │   ├── jassi
    │   └── master
    └── tags
```

When user add file(s) with `git add <fileName>` also known as `staging`. A new file (blob type) is created inside `.git/objects`

```bash
.git/objects
├── 0b
│   └── 2963a7855ab412c8838cc1883802f6588c4d2a
├── 2d
│   └── 15f810f1b14e0c41560bd19aea42db704bcc8c
├── 6f
│   └── 0d725631453bc11db9e31017a68488dd7cd99e
├── bf
│   └── 6b8179632b5711ffa21a9869186fae9199d99c
├── e6
│   └── 9de29bb2d1d6434b8b29ae775ad8c2e48c5391
├── f0
│   └── ad0dec66b594ef1f8bdd71081bdad977b828d4
├── info
└── pack
```

When user commits the staging changes with `git commit ...`. 2 new files are created in `.git/objects`, i.e a commit file and a tree file.

- Commit file includes commit information about commit as follows.

```bash
git cat-file -p 6f0d
# where 0b is folder name and 2d is first 2 characters of objectName.
```

Contents of 6f0d - commit file

```txt
tree 2d15f810f1b14e0c41560bd19aea42db704bcc8c
parent 6f0d725631453bc11db9e31017a68488dd7cd99e
author Jaspreet Singh -- fearless <jaspreetsingh2379@gmail.com> 1576499527 +0530
committer Jaspreet Singh -- fearless <jaspreetsingh2379@gmail.com> 1576499527 +0530

Commit #2
```

- Tree: This is name/id of file related to this commit.
- Parent: This is name/id of file, parent of this commit(For 1st commit, parent not exist).
- Note: If parent is missing, that commit is first commit.
- author and comitter info
- timestamp and timezone
- commit message

```bash
git cat-file -p 2d15
# where 2d15 is tree included in commit above.
```

```txt
000644 blob bf6b8179632b5711ffa21a9869186fae9199d99c    hello.js
100644 blob f0ad0dec66b594ef1f8bdd71081bdad977b828d4    hello.txt
```

Tree file include following structure of lines

`permissions` `type` `blob id` `fileName`

---

### HEAD

`.git/HEAD` is a regular file.

Run

```bash
cat .git/HEAD
```

outputs

```txt
ref: refs/heads/master
```

---

### REFS

`.git/refs` contains following info.

```bash
# Run
tree .git/refs
```

outputs

```bash
.git/refs
├── heads
│   ├── master
│   └── feature
└── tags
```

Contents of `.git/HEAD` reference or point to `.git/refs/heads/branch-name`.

```bash
#execute
cat .git/refs/heads/master
```

outputs

```txt
6f0d725631453bc11db9e31017a68488dd7cd99e
```

Contents of `.git/refs/heads/branch-name` point to object type commit `6f0d`

---

## Summary

`.git/HEAD` &rightarrow; `.git/refs/heads/master` &rightarrow; `Commit File` &rightarrow; `Tree File` &rightarrow; `Blobs`

_(above strucuture is like an linked list)_

1. `.git/HEAD`: Regular file include ref to .git/refs/heads/branch
2. `.git/refs/heads/branch`: regular file include id of commit file
3. `Commit File`: Include tree file id, parent commit file id, author, committer, commit message.
4. `Tree File`: Include multiple lines representing `permission type blobId filename`
5. `Blob File`: Include content of a file, when added in staging by user.

## Tips

- As soon as files are staged, new object with random id (2d0a....) is created.
- On a commit, latest version of each staged file is stored in commit file. Earlier versions, say if executed git add multiple times between 2 commits, will keep all versions in `.git` folder.
- Branches (`.git/refs/heads/branch`) are just a pointer to a commit file. Thus, Creating branches are easy and cheap.
- HEAD points on one of `.git/refs/heads/branch`.

### HEAD change flow

When branches are switched (or created), `.git/HEAD` stores ref to new branch `.git/refs/heads/branchName`. Earlier reference is cleared. But a history of HEAD changes are maintained inside `.git/logs/HEAD`.

#### Logs

```bash
tree .git/logs
```

outputs

```bash
.git/logs
├── HEAD
└── refs
    └── heads
        ├── master
        └── feature
```

Contents of `.git/logs/HEAD`

```bash
# run
cat .git/logs/HEAD
```

outputs, (some text truncated for simplicity)

```txt
00000... 6f0d7... John Doe <johndoe@gmail.com> 1576493024 +0530 commit (initial): Commit 1
6f0d7... 6f0d7... John Doe <johndoe@gmail.com> 1576494066 +0530 checkout: moving from master to feature
6f0d7... 6f0d7... John Doe <johndoe@gmail.com> 1576494111 +0530 checkout: moving from feature to master
6f0d7... 17892... John Doe <johndoe@gmail.com> 1576499527 +0530 commit: Commit #2
```

Each lines include following pattern:

`a b c d e f x:y`

- `a` - Commit Id before change
- `b` - Commit Id after change
- `c` - Author Name
- `d` - Author Email
- `e` - Timestamp
- `f` - TimeZone
- `x:y`
  - `x`: Type of change i.e `commit(initial)`,`commit`, `checkout`
  - `y`: Commit Message or `moving from branchA o branch B`

For initial Commit id is `000000000000...`

Contents of `.git/logs/refs/heads/branch`

```bash
# run
cat .git/logs/refs/heads/branch
```

outputs, (some text truncated for simplicity)

```txt
00000... 6f0d7... John Doe <johndoe@gmail.com> 1576493024 +0530  commit (initial): Commit 1
6f0d7... 17892... John Doe <johndoe@gmail.com> 1576499527 +0530  commit: Commit #2
```

Each lines include following pattern as `.git/logs/HEAD`.

### Deletion

If a branch is deleted, `.git/refs/heads/branch` is removed. Thus, only the pointer to commit file
is removed. The commit file is now orphan.

If a commit file is deleted, `commit file` in `.git/objects` is removed. Thus, only the pointer to tree file, parent commit and commit msg are removed. Tree File is now orphan.
??: does `.git/refs/heads/branch` point to parent commit.
