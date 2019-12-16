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

### TABLE OF CONTENTS

- [1. Intro](README.md)
- [2. Staging](2.Staging.md)
- [3. .git/HEAD](3.HEAD.md)
- [4. .git/refs/head/branch](4.refs.md)
- [5. Summary](5.Summary.md)
- [6. .git/logs](6.Logs.md)
- [7. Deletion](7.Deletion.md)
- [8. Git Merge](8.Merge.md)
- [9. Git Rebase](9.Rebase.md)