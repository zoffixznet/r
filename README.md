
# How to build Rakudo from Source

## Linux

These instructions assume you're using some sort of Linux, like
[Debian](https://www.debian.org/) and `bash` for shell.

You'll need `git` and a C compiler, which on Debian you can get by running:

```bash
    sudo apt-get update
    sudo apt-get -y install build-essential git
```

Next, we'll close the source code repository to `~/rakudo`, then add a couple
of paths to `PATH` as well as create a bash alias to update to latest
development commit.

```bash
    git clone https://github.com/rakudo/rakudo/ ~/rakudo
    echo 'export PATH="$HOME/rakudo/install/bin:$HOME/install/share/perl6/site/bin:$PATH"' >> ~/.bashrc
    echo 'alias update-perl6='\''
        cd ~/rakudo;
        git pull;
        perl Configure.pl --gen-moar --gen-nqp --backends=moar;
        make;
        make test;
        make install'\''' >> ~/.bashrc
    source ~/.bashrc
```

You're now all set! To upgrade to latest development commit of Rakudo, simply
type `update-perl6` in your shell and wait for the build process to complete. It takes a dozen minutes and needs about of 1.4GB of RAM (swap will work too).

**IMPORTANT:** note that these instructions build latest development commits
that undergo minimal amount of testing and may contain severe bugs.


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
    perl Configure.pl --gen-moar --gen-nqp --backends=moar & gmake & gmake test & gmake install
```

Add paths to the executables by [editing `PATH` environmental
variable](https://www.google.com/search?q=windows+edit+environmental+variables&ie=utf-8&oe=utf-8). To its current value, add the following:

    ;C:\rakudo\install\bin;C:\rakudo\install\share\perl6\site\bin

To update to latest development commit, simply navigate to the repository
check out, pull latest changes, and run the build command again:

```cmd
    C:
    cd \rakudo
    git pull
    perl Configure.pl --gen-moar --gen-nqp --backends=moar & gmake & gmake test & gmake install
```