# dsnapshot #

A command that uses rsnapshot to make incremental versions of a directory on
the same volume.

For usage instructions, run `dsnapshot --help`


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
mkdir /usr/share/dsnapshot
chmod a+rx /usr/share/dsnapshot
cp template-rsnapshot.conf /usr/share/dsnapshot
chmod a+rx /usr/share/dsnapshot/template-rsnapshot.conf
```
