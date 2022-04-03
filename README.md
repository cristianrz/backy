# backy

Backs up a specific directory incrementally using only POSIX shell and `rsync`
using a JBOF (Just Bunch Of Files) approach.

Creates the following structure:

```
backy
├── daily
│   └── 2022-04-03
├── last -> yearly/2022
├── monthly
│   └── 2022-04
└── yearly
    └── 2022
```

When ran, it will do the following:

* Backup the specified directory to a directory with the current date as a name
inside `daily`
* BDestination /mnt/e/backy
Source /mnt/c/Users/cristianackup the specified directory to a directory with the current date as a name
inside `weekly`
* Backup the specified directory to a directory with the current date as a name
inside `yearly`

The link named `last` will always point to the last backup done. The next
backup will use hard links to point to already existing files on the previous 
backup to save space.

Config resides inside `$HOME/.config/backy` and contains two files, `config`
and `excludes`.

`config` contains the source and destination directory:

```
Destination /var/backups/backy
Source /home/jimmy
```

`excludes` contains the excludes directories:

```
.cache
Downloads
```

There is no automatic pruning of old archives (for now), although it is trivial
to prune old archives since by opening the `daily` directory you can see all
the daily archives and all the archives you don't need.

There is no encryption build in, `rclone` can be used for that.
