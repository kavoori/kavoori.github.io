---
layout: post
title:  "Apache crashing with seg fault on a graceful restart? It might be because of a module that is enabled."
date:   2015-03-21 20:40:00
comments : true
categories: Blog
---
Is your Apache 2 server crashing with the error `"AH00060: seg fault or similar nasty error detected in the parent process"`? Did you think log_rotate was somehow involved?

I have noticed recently (after a number of upgrades on my Ubuntu machine) that the Apache 2 server would periodically shut down. I looked at the `/var/log/apache2/error.log` and sure enough it crashed with the following error:

{% highlight bash %}
[core:notice] AH00060: seg fault or similar nasty error detected in the parent process
{% endhighlight %}

All googling pointed at `log_rotate` being the offender as the shut down would always happen at the exact time (maybe each Sunday morning to rotate the apache logs) for most people. I looked at `/etc/logrotate.d/apache2` file and it sure was restarting apache to rotate the logs.

{% highlight bash linenos %}
/var/log/apache2/*.log {
	weekly
	missingok
	rotate 52
	compress
	delaycompress
	notifempty
	create 640 root adm
	sharedscripts
	postrotate
                if /etc/init.d/apache2 status > /dev/null ; then \
                    /etc/init.d/apache2 reload > /dev/null; \
                fi;
	endscript
	prerotate
		if [ -d /etc/logrotate.d/httpd-prerotate ]; then \
			run-parts /etc/logrotate.d/httpd-prerotate; \
		fi; \
	endscript
}
{% endhighlight %}

I changed line 12 above to `restart` instead of `reload` and that stopped apache from shutting down periodically every time the logs had to rotate.

Now it was time to investigate why a `reload` is crashing apache. A quick look at `/etc/init.d/apache2` script showed that `reload` is just a wrapper for `apache2ctl graceful` 

{% highlight bash %}
.....
reload|force-reload|graceful)
        log_daemon_msg "Reloading $DESC" "$NAME"
        do_reload
        RET_STATUS=$?

.....    

do_reload() {
        if apache_conftest; then
                if ! pidofproc -p $PIDFILE "$DAEMON" > /dev/null 2>&1 ; then
                        APACHE2_INIT_MESSAGE="Apache2 is not running"
                        return 2
                fi
                $APACHE2CTL graceful > /dev/null 2>&1
                return $?
        else
                APACHE2_INIT_MESSAGE="The apache2$DIR_SUFFIX configtest failed. Not doing anything."
                return 2
        fi
}    
{% endhighlight %}

Fortunately, I had a different machine where this was not happening. I got a list of modules that are enabled on that machine and eliminated them from the list of suspects. Starting with each of the remaining enabled modules, I would do the following : 

1. Disable the module with `a2dismod`
   Example : `a2dismod access_compat`
2. Restart apache2 for the above change to take effect
   `service apache2 restart`
3. Check that apache2 actually came up after the restart
    `ps -ef | grep apache2 | grep -v grep | wc`
4. Now use the  command that is causing the crash
   `service apache2 graceful`
5. Check again if apache2 is running
    `ps -ef | grep apache2 | grep -v grep | wc`
6. If apache is still down, then the above module is not causing it, so re-enable it using `a2enmod` and restart
   `a2enmod access_compat && service apache2 restart`
7. Check that apache2 came up after enabling the module back and restarting
	`ps -ef | grep apache2 | grep -v grep | wc`
8. Repeat with the next enabled module.

I did this until I found the offender : `mime_magic`

tl;dr
`sudo a2dismod mime_magic && sudo service apache2 restart`



