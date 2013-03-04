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

	#!/bin/sh
	# For RedHat and cousins:
	# chkconfig: 2345 85 85
	# description: Red5 flash streaming server
	# processname: red5
	PROG=red5
	RED5_HOME=/usr/local/red5
	DAEMON=$RED5_HOME/$PROG.sh
	PIDFILE=/var/run/$PROG.pid
	# Source function library
	. /etc/rc.d/init.d/functions
	[ -r /etc/sysconfig/red5 ] && . /etc/sysconfig/red5
	RETVAL=0
	case “$1″ in
	start)
	echo -n $”Starting $PROG: ”
	cd $RED5_HOME
	$DAEMON >/dev/null 2>/dev/null &
	RETVAL=$?
	if [ $RETVAL -eq 0 ]; then
	echo $! > $PIDFILE
	touch /var/lock/subsys/$PROG
	fi
	[ $RETVAL -eq 0 ] && success $”$PROG startup” || failure $”$PROG startup”
	echo
	;;
	stop)
	echo -n $”Shutting down $PROG: ”
	killproc -p $PIDFILE
	RETVAL=$?
	echo
	[ $RETVAL -eq 0 ] && rm -f /var/lock/subsys/$PROG
	;;
	restart)
	$0 stop
	$0 start
	;;
	status)
	status $PROG -p $PIDFILE
	RETVAL=$?
	;;
	*)
	echo $”Usage: $0 {start|stop|restart|status}”
	RETVAL=1
	esac
	exit $RETVAL



*Now start the service*

`/etc/init.d/red5 start`


*Check the server status:*

	/etc/init.d/red5 status
	red5 (pid  XXXXX) is running…



###STEP 4

*Now test the RED5 installation by opening following URL in browser:*
`http://yourip:5080/`

That's it, enjoy hacking away on Red5 Server!
