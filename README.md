## Scrutiny

Server report collection tool using ps, top, df, vmstat, iostat, netstat,
mysqladmin, and apache's server-status module to help create a point in
time snapshot of systems events for troubleshooting purposes.


### Purpose

Being asked at 9AM to determine what caused a system to have problems at
2:30AM can be a weary task.  If the normal system logs do not give us any
real hints about what may have caused the issue, we oftentimes get trapped
having to give the really poor answer of "We cannot replicate the issue
that you experienced during the overnight, and the logs are not giving us
enough information to go on.  So we'll have to watch for it tonight to see
if it re-occurs."  Times like that makes a sysadmin feel completely
helpless.

What if you could see what processes were running on the system at
prescribed intervals?  And not just processes, but what about what queries
were running, how many people were hitting Apache, perhaps what types of
network connections you were getting, on top of a bunch of other
information that can be gathered from tools like vmstat, iostat, etc?  Now
you can draw better conclusions cause you will know what was happening at
that single point in time.

Welcome Scrutiny!  A tool based off of
[recap](https://github.com/rackerlabs/recap), rewriten to suit my own
needs for portability between Red Hat, Debian, and FreeBSD based systems,
as well as allowing for simple modifications of the metrics needed to best
suit your own environment.


### Features

- Simple code base for quick customizations
- Ability to enable/disable groups of checks
- Easy to add/modify/remove individual metric gathering
- Uses tools such as ps, top, df, vmstat, iostat, netstat, mysqladmin, and
  apache's server-status module to help create a point in time snapshot of
the systems events.


### Configuration

The currently configurable options and thresholds are listed below:

	# Enable / Disable Statistics
	process_log=on
	resource_log=on
	network_log=on
	mysql_log=on
	apache_log=on
	nginx_log=off

	# Retention Days
	retension=2

	# Logs
	basedir=/var/log/scrutiny


### Implementation

Download script to desired directory and set it to be executable:

	# Linux based systems
	cd /root
	git clone https://github.com/stephenlang/scrutiny
	chmod 755 /root/scrutiny/linux/scrutiny.sh
	
	# FreeBSD based systems
	cd /root
	git clone https://github.com/stephenlang/scrutiny/freebsd/scrutiny.sh
	chmod 755 /root/scrutiny/freebsd/scrutiny.sh

After configuring the tunables in the script (see above), create a cron job
to execute the script every 10 minutes:

	# Linux based systems
	crontab -e
	*/10 * * * * /root/scrutiny/linux/scrutiny.sh

	# FreeBSD based systems
	crontab -e
	*/10 * * * * /root/scrutiny/freebsd/scrutiny.sh

Now days later, if a problem was reported during the overnight and you were
able to narrow it down to a specifc timeframe, you will be able to look at
the point in time snapshots of system events that occured:
ls /var/log/scrutiny
