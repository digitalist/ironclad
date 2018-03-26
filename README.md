@kmoore134
Ok, Here's my initial brain-dump. Not at a system with the information in front of me, so forgive any mistakes / omissions:

    Setup

Since we build on HEAD and poudriere is essentially using jails to do builds, you need to be running a system with a newer version of HEAD than the version of the OS you plan on building. I.E. trueos-unstable can build trueos-stable/trueos-unstable branches, but trueos-stable often cannot build trueos-unstable packages.

    Install poudriere

Since we're building with poudriere, we obviously need to install it. "pkg install poudriere" will get you setup. Once thats done edit /usr/local/etc/poudriere.conf to ensure the correct build settings are used, since its pretty easy to swap a builder to death if you use too many builders and do not have enough ram.

Once the initial setup is done, create a poudriere jail using the distfiles for whatever branch of TrueOS you intended to build on top of. Those files are located on the download site:

http://download.trueos.org/unstable/amd64/dist/

# poudriere jail -c -j trueos -v 12-CURRENT -m url=http://download.trueos.org/unstable/amd64/dist/

(To fix an issue with building using OpenRC there is a slight modification we have to make to poudriere, this may go away in the future when I or somebody has time to fix the "service" command working in openrc jails)
https://github.com/iXsystems/ixbuild/blob/master/trueos/scripts/portbuild.sh#L180

    The ports repo

The ports tree we build is https://github.com/trueos/freebsd-ports which we can setup with the following:

# echo "GIT_URL=https://github.com/trueos/freebsd-ports" >> /usr/local/etc/poudriere.conf
# poudriere ports -c -p trueos-ports -B trueos-master -m git

    Starting the build

First, grab a copy of our master ports make knobs:

https://github.com/trueos/trueos-core/blob/master/build-files/conf/desktop/port-make.conf

# cp port-make.conf /usr/local/etc/poudriere.d/trueos-make.conf

Now you can go ahead and start a complete build:

# poudriere bulk -a -j trueos -p trueos-ports
# sudo poudriere bulk  -j trueos -p trueos-ports -a games/oldrunner
# sudo poudriere bulk  -j trueos -p trueos-ports games/oldrunner

Of course you can replace -a with a list of ports to build if you don't want to do all 28k~

Note: This is all ripped from our build system here: https://github.com/iXsystems/ixbuild/tree/master/trueos/scripts

That repo is quite a bit of a mess right now and in desperate need of a re-write, so if somebody wants to take on the task of building a much "simpler" solution I'd be very happy to switch to it ;)