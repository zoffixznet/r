# Table of Contents

- [Linux](#linux)
- [Windows](#windows)
- [Module Manager](#module-manager)
- [Inline::Perl5 with Latest Perl](#inlineperl5-with-latest-perl)


# How to build development version of Rakudo from source code

These instructions explain how to build latest *development* version
of the [Rakudo compiler](https://perl6.org/). For latest *release*
CentOS, Debian, Fedora, and Ubuntu rakudo packages, see
[https://github.com/nxadm/rakudo-pkg/releases](https://github.com/nxadm/rakudo-pkg/releases).
For latest release of Rakudo Star distribution, see
[https://rakudo.perl6.org/downloads/star/](https://rakudo.perl6.org/downloads/star/).
If you don't know what you want, you want Rakudo Star.

**IMPORTANT:** note that these instructions build latest development commits
that undergo minimal amount of testing and may contain severe bugs.

## Linux

These instructions assume you're using some sort of Linux, like
[Debian](https://www.debian.org/), and `bash` for shell.

You'll need `git` and a C compiler, which on Debian you can get by running:

```bash
sudo apt-get update
sudo apt-get -y install build-essential git
```

Next, we'll clone the source code repository to `~/rakudo`, then add a couple
of paths to `PATH` as well as create a bash alias to update to latest
development commit.

```bash
git clone https://github.com/rakudo/rakudo/ ~/rakudo
echo 'export PATH="$HOME/rakudo/install/bin:$HOME/rakudo/install/share/perl6/site/bin:$PATH"' >> ~/.bashrc
echo 'alias update-perl6='\''
    cd ~/rakudo && git pull &&
    perl Configure.pl --gen-moar --gen-nqp --backends=moar &&
    make && make install'\''' >> ~/.bashrc
source ~/.bashrc

# Now just run the following to install rakudo or to upgrade it to latest dev commit in the future
update-perl6
```

You're now all set! To upgrade to latest development commit of Rakudo, simply
run `update-perl6` in your shell and wait for the build process to complete.
It takes a dozen minutes and needs about of 1.4GB of RAM (swap will work too).
You may want to [install module manager](#module-manager) next.

If you ever want to do a "from scratch" update by deleting `~/rakudo` checkout,
be sure to also delete some of the directories used by the module manager:

```bash
rm -fr ~/.zef ~/.perl6
```

## Windows

Download and install [Git](https://git-scm.com/download/win) as well as
[Strawberry Perl](http://strawberryperl.com/), which includes Perl,
a C compiler, and other relevant utilities.

Press `Win+R` and run `cmd` command. Then navigate where you want to clone
rakudo. The following clones to `C:\rakudo\` directory:

```cmd
C:
mkdir \rakudo
cd \rakudo
git clone https://github.com/rakudo/rakudo/ .
```

Then build rakudo (`gmake` might be called something else, like `nmake` if
you installed a different C compiler):

```cmd
perl Configure.pl --gen-moar --gen-nqp --backends=moar & gmake & gmake install
```

Add paths to the executables by [editing `PATH` environmental
variable](https://www.google.com/search?q=windows+edit+environmental+variables&ie=utf-8&oe=utf-8). To its current value, add the following (alter the paths if you installed to somewhere other than `C:\rakudo\`):

```
;C:\rakudo\install\bin;C:\rakudo\install\share\perl6\site\bin
```

To update to latest development commit, simply navigate to the repository
check out, pull latest changes, and run the build command again:

```cmd
C:
cd \rakudo
git pull
perl Configure.pl --gen-moar --gen-nqp --backends=moar & gmake & gmake test & gmake install
```

## Module Manager

After installing rakudo, install the module manager. Simply run the
following in any temporary directory (after installation, directory
can be deleted):

```bash
git clone https://github.com/ugexe/zef
cd zef
perl6 -I. bin/zef install .
```

You do *not* need to re-install `zef` after upgrading to newer versions of rakudo.
To *upgrade* `zef`, simply run `zef upgrade zef`

## Inline::Perl5 with Latest Perl

To install [`Inline::Perl5`](http://modules.perl6.org/repo/Inline::Perl5) with
latest Perl, you can use the following (instructions for Linux):

```bash
\curl -L https://install.perlbrew.pl | bash
echo 'source ~/perl5/perlbrew/etc/bashrc' >> ~/.bashrc
source ~/.bashrc
perlbrew install perl-5.26.1 --notest -Duseshrplib -Dusemultiplicity
perlbrew switch perl-5.26.1
perlbrew install-cpanm

zef install Inline::Perl5
```
