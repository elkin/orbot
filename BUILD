This document explains how to properly build an Android package of Orbot from
source.

Please install the following prerequisites (instructions for each follows):
	Android OS SDK
	droid-wrapper: http://github.com/tmurakam/droid-wrapper
	libevent source
	Tor source (most recent git master branch)

Install and prepare the Android OS SDK ( http://source.android.com/download )
on Debian Lenny:

	sudo apt-get install git-core gnupg sun-java5-jdk flex bison gperf \
		libsdl-dev libesd0-dev libwxgtk2.6-dev build-essential zip \
		curl libncurses5-dev zlib1g-dev valgrind
	update-java-alternatives -s java-1.5.0-sun

	curl http://android.git.kernel.org/repo >~/bin/repo
	chmod a+x ~/bin/repo

	mkdir ~/mydroid
	cd ~/mydroid

	repo init -u git://android.git.kernel.org/platform/manifest.git
	repo sync

	# Paste in key from http://source.android.com/download next...
	gpg --import

	cd ~/mydroid

	# This takes a long while...
	make

Install droid-wrapper:

	cd /tmp
	git clone git://github.com/tmurakam/droid-wrapper.git
	cd droid-wrapper
	sudo make install

zlib and OpenSSL are included with the Android OS SDK. You'll need to build
libevent and finally Tor. We'll create an externals directory for this code:

	mkdir -p ~/mydroid/external/{libevent,tor}

We need to set to environment variables for droid-gcc:
	export DROID_ROOT=~/mydroid/
	export DROID_TARGET=generic

Fetch and build libevent:

	cd ~/mydroid/external/libevent
	svn co https://levent.svn.sourceforge.net/svnroot/levent .
	cd trunk/libevent/
	export LIBEVENTDIR=`pwd`
	./autogen.sh
	CC=droid-gcc LD=droid-ld ./configure --host=arm-none-linux-gnueabi
	make

Fetch and build Tor:

	export OPENSSLDIR=`cd ~/mydroid/external/openssl/include/ && pwd`
	export ZLIBDIR=`cd ~/mydroid/external/zlib && pwd`

	cd ~/mydroid/external/tor
	git clone https://git.torproject.org/git/tor.git
	cd tor/
	CC=droid-gcc LD=droid-ld ./configure --host=arm-none-linux-gnueabi \
	--with-libevent-dir=$LIBEVENTDIR --with-openssl-dir=$OPENSSLDIR \
	--with-zlib-dir=$ZLIBDIR
	make

At this point, you'll have a Tor binary that can be run on an Android handset.
This isn't enough though and we'll now sew up the binary into a small package
that will handle basic Tor controlling features.

XXX TODO: Explain build process for making a .apk file for install.