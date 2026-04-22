# backy

Incremental backups using only POSIX shell and `rsync`.

Uses hardlinks to deduplicate unchanged files across snapshots — full
backup semantics at a fraction of the space.

## How it works

Each run creates three snapshots: daily, monthly, and yearly. A `last`
symlink always points to the most recent backup and is used as the
hardlink base for the next run, so only changed files take new space.

```
/var/backups/backy
├── daily
│   ├── 2024-03-01
│   └── 2024-03-02
├── monthly
│   └── 2024-03
├── yearly
│   └── 2024
└── last -> daily/2024-03-02
```

Old snapshots are pruned automatically: 30 dailies, 12 monthlies, 5 yearlies.

## Installation

```sh
cp backy ~/.local/bin/backy
chmod +x ~/.local/bin/backy
```

Then add it to your crontab:

```
@daily backy
```

## Configuration

Config lives in `~/.config/backy/`.

**`~/.config/backy/config`** — source and destination:

```
Source /home/jimmy
Destination /var/backups/backy
```

**`~/.config/backy/excludes`** — directories to skip, one per line:

```
.cache
Downloads
```

## Encryption

Not built in. Pipe the destination through
[rclone](https://rclone.org/crypt/) for encrypted remote storage.

## License

BSD 3-Clause
