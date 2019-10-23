# dsnapshot — Directory Snapshot #

A command that uses rsnapshot to make incremental versions of a directory on
the same volume.

[rsnapshot](http://rsnapshot.org/) handles efficient backups well. It only
writes the files that have changed since the last backup and hard links the
rest from there.

What rsnapshot does not do is let you simply run it ad‐hoc with command‐line
options to make snapshots of any directory. It also doesn’t hard link files
from the source directory so that you can make efficient snapshots on the same
volume that share data with the original source.

That’s where dsnapshot comes in.

Basic use:

```bash
dsnapshot /path/to/source-directory /path/to/snapshots-directory
```

That will clone every file in `source-directory` into the `snapshots-directory`
reusing the same data on disk (using hard links). The next time you run it, it
will create a snapshot of the last backup in `snapshots-directory`, then
hard‐link only the files that have changed into that.

Each time you run it, dsnapshot creates a `daily` snapshot in the snapshots
directory. If it is the first day of the week (Sunday) it will create a new
`weekly` snapshot. If it is the first day of the month it will create a
`monthly` snapshot.

For full usage instructions, run `dsnapshot --help`


# Installation #

First, you need to install [rsnapshot](http://rsnapshot.org/). It’s in the
package repositories for most major Linux distributions.

For a local user install you can symlink `dsnapshot` to somewhere on your path,
like this:

```bash
ln -s /path/to/dsnapshot ~/.local/bin/dsnapshot
```

For a system wide install, do the following as root.

```bash
# Put the command in place
cp /path/to/dsnapshot /usr/bin/dsnapshot
chmod a+x /usr/bin/dsnapshot

# Put the rsnapshot config template into place
mkdir /etc/dsnapshot
chmod a+rx /etc/dsnapshot
cp template-rsnapshot.conf /etc/dsnapshot
chmod a+rx /etc/dsnapshot/template-rsnapshot.conf
```


## Configuration ##

You don’t need to configure this if you don’t want to, but you may want to
change the rsnapshot configuration template file it uses. This is in your
`template-rsnapshot.conf` file.

It will look for your `template-rsnapshot.conf` file in the following locations
and use the first one it finds.

1. In the same directory as your `dsnapshot` command.
1. At `/etc/dsnapshot/template-rsnapshot.conf`
1. At `/usr/share/dsnapshot/template-rsnapshot.conf`

This is a standard rsnapshot config file with only a few differences. There are
some settings in the file that you should not edit. In the `BACKUP POINTS /
SCRIPTS` section there is only one backup point defined. Leave this as is.

In adition, do not edit any of the following settings.

* `config_version`
* `snapshot_root`
* `cmd_rsync` —unless your system’s `rsync` command is actually in a different place
* `rsync_short_args`
* `rsync_long_args`

What you may want to edit are the `BACKUP INTERVALS`. Any valid options are
fine here, but be aware that dsnapshot only runs backup intervals with the
names `daily`, `weekly`, and `monthly`. You can change the number of snapshots
to keep for each of those, but creating any other intervals will do nothing.
