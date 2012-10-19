Oracle Java 6 On Debian
=======================

Rationale
---------
 * Most of our software didn't work with OpenJDK on debian stable `Squeeze`.
 * All our software is tested and works with Oracle Java 6.
 * It's much less work to backport oracle java as all the QA has
      already been done.


Build Instructions
------------------
- Download oracle sun java 6 binary packages (.bin NOT .rpm) for both 32 and 64 bit
- Rename the jdk to use amd64 instead of x64 as architecture (eg. mv jdk-6u30-linux-x64.bin jdk-6u30-linux-amd64.bin)
- Create .orig.tar.gz file (e.g sun-java6_6.30.orig.tar.gz)

    % tar tf sun-java6_6.30.orig.tar.gz
    sun-java6-6.30/
    sun-java6-6.30/jdk-6u30-linux-amd64.bin
    sun-java6-6.30/jdk-6u30-linux-i586.bin

- Copy both .bin files into the repository too
- Rename repository from sun-java6 to sun-java6-6.RELEASE (e.g sun-java6-6.30, you can also just use git-new-workdir)

    git new-workdir sun-java6 sun-java6-6.30

- debuild -uc -us -i'(.git|README)'
- debuild -uc -us -i'(.git|README)' -ai386 -B   (to build the 32bit version, produces wrong dependencies if build on amd64)


Install Dependencies
--------------------
If you build your java package without chroot/pbuilder/cowbuilder you will need to install a few debian packages.
For a complete list please see the file `debian/control`.

- Debian packaging: apt-get install build-essential devscripts debhelper
- Sun-java6 build dependencies: apt-get install defoma libasound2 unixodbc libxi6 libxt6 libxtst6 lib32asound2 ia32-libs


Changelog
---------

*6u37*:

- security upgrade due to unintended remote management features

*6u31*:

- google appengine apps do not start anymore, timezone _defaultZoneTL_ was removed in 6u31 (http://code.google.com/p/googleappengine/issues/detail?id=6928)

*6u30*:

- no known problems
