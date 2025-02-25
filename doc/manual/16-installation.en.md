\newpage

# Installation

> **IMPORTANT:**
> The recommended way to install Barman is by using the available
> packages for your GNU/Linux distribution.

## Installation on RedHat/CentOS using RPM packages

Barman can be installed on RHEL7 and RHEL6 Linux systems using
RPM packages. It is required to install the Extra Packages Enterprise
Linux (EPEL) repository and the
[PostgreSQL Global Development Group RPM repository][yumpgdg] beforehand.

Official RPM packages for Barman are distributed by EnterpriseDB
via Yum through the [public RPM repository][2ndqrpmrepo],
by following the instructions you find on that website.

Then, as `root` simply type:

``` bash
yum install barman
```

> **NOTE: **
> We suggest that you exclude any Barman related packages from getting updated
> via the PGDG repository. This can be done by adding the following line
> to any PGDG repository definition that is included in the Barman server inside
> any `/etc/yum.repos.d/pgdg-*.repo` file:
   ```ini
   exclude=barman* python*-barman
   ```
> By doing this, you solely rely on
> EnterpriseDB repositories for package management of Barman software.

For historical reasons, EnterpriseDB keeps maintaining package distribution of
Barman through [Sourceforge.net][3].

## Installation on Debian/Ubuntu using packages

Barman can be installed on Debian and Ubuntu Linux systems using
packages.

It is directly available in the official repository for Debian and Ubuntu, however, these repositories might not contain the latest available version.
If you want to have the latest version of Barman, the recommended method is to install both these repositories:

* [Public APT repository][2ndqdebrepo], directly maintained by
  Barman developers
* the [PostgreSQL Community APT repository][aptpgdg], by following instructions in the [APT section of the PostgreSQL Wiki][aptpgdgwiki]

> **NOTE:**
> Thanks to the direct involvement of Barman developers in the
> PostgreSQL Community APT repository project, you will always have access
> to the most updated versions of Barman.

Installing Barman is as easy. As `root` user simply type:

``` bash
apt-get install barman
```

## Installation from sources

> **WARNING:**
> Manual installation of Barman from sources should only be performed
> by expert GNU/Linux users. Installing Barman this way requires
> system administration activities such as dependencies management,
> `barman` user creation, configuration of the `barman.conf` file,
> cron setup for the `barman cron` command, log management, and so on.

Create a system user called `barman` on the `backup` server.
As `barman` user, download the sources and uncompress them.

For a system-wide installation, type:

``` bash
barman@backup$ ./setup.py build
# run this command with root privileges or through sudo
barman@backup# ./setup.py install
```

For a local installation, type:

``` bash
barman@backup$ ./setup.py install --user
```

The `barman` application will be installed in your user directory ([make sure that your `PATH` environment variable is set properly][setup_user]).

[Barman is also available on the Python Package Index (PyPI)][pypi] and can be installed through `pip`.

# Upgrading Barman

Barman follows the trunk-based development paradigm, and as such
there is only one stable version, the latest. After every commit,
Barman goes through thousands of automated tests for each
supported PostgreSQL version and on each supported Linux distribution.

Also, **every version is back compatible** with previous ones.
Thefore, upgrading Barman normally requires a simple update of packages
using `yum update` or `apt update`.

There have been, however, the following exceptions in our development
history, which required some small changes to the configuration.

## Upgrading from Barman 2.10

If you are using `barman-cloud-wal-archive` or `barman-cloud-backup`
you need to be aware that from version 2.11 all cloud utilities
have been moved into the new `barman-cli-cloud` package.
Therefore, you need to ensure that the `barman-cli-cloud` package
is properly installed as part of the upgrade to the latest version.
If you are not using the above tools, you can upgrade to the latest
version as usual.

## Upgrading from Barman 2.X (prior to 2.8)

Before upgrading from a version of Barman 2.7 or older
users of `rsync` backup method on a primary server should explicitly
set `backup_options` to either `concurrent_backup` (recommended for
PostgreSQL 9.6 or higher) or `exclusive_backup` (current default),
otherwise Barman emits a warning every time it runs.

## Upgrading from Barman 1.X

If your Barman installation is 1.X, you need to explicitly configure
the archiving strategy. Before, the file based archiver, controlled by
`archiver`, was enabled by default.

Before you upgrade your Barman installation to the latest version,
make sure you add the following line either globally or for any server
that requires it:

``` ini
archiver = on
```

Additionally, for a few releases, Barman will transparently set
`archiver = on` with any server that has not explicitly set
an archiving strategy and emit a warning.
