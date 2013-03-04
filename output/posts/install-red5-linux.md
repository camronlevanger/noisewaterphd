.. title: Installing Red5 Server on Linux (CentOS)
.. slug: install-red5-linux
.. date: 2010/04/28 11:51:56
.. tags: programming, devops, Red5, Java, Linux, CentOS

Red5 Installation Instructions
------------------------------

**NONE OF THIS WILL WORK WITHOUT ANT, So first install ANT > 1.8.0**

###STEP 1

Once ANT is installed we can download and install the Red5 server:
We need Red5 version 0.9.RC1 specifically in order to have streaming work properly for the web phone.

	cd /usr/src
	svn checkout http://red5.googlecode.com/svn/java/server/tags/0_9rc1/ red5
	mv red5 /usr/local/
	cd /usr/local/red5
	ant prepare
	ant dist

*Once the compile is finished you should see:*
`BUILD SUCCESSFUL`

###STEP 2

Now we need to set up the configuration directory: copy the conf directory from dist:

	cp -r dist/conf .
	./red5.sh

###STEP 3

If everything starts up nicely, then we can build an init script for Red5:

`vi /etc/init.d/red5`

Copy this text in:

<script src="https://gist.github.com/camronlevanger/5080937.js"></script>



*Now start the service*

`/etc/init.d/red5 start`


*Check the server status:*

	/etc/init.d/red5 status
	red5 (pid  XXXXX) is running…



###STEP 4

*Now test the RED5 installation by opening following URL in browser:*
`http://yourip:5080/`

That's it, enjoy hacking away on Red5 Server!
