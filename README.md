# OpenWrt send mail
1. opkg update;opkg install mutt msmtp

2.edit file  /etc/msmtprc:
	
	account default
	 
	host smtp.126.com
	auth login
	user qq859755014
	password Pa55word
	auto_from off
	from  qq859755014@126.com
	syslog LOG_MAIL
	logfile /var/log/msmtp.log

3.edit file   ~/muttrc

	set charset="utf-8"
	
	# incoming mail boxes
	mailboxes "=inbox"
	
	# mail account setup.
	set sendmail="/usr/bin/msmtp"
	set from="qq859755014@126.com"
	
	
	# mail folder setup.
	set folder="/root/Mail/mail"
	set spoolfile="/root/Mail/mail/inbox"
	set mbox="/root/Mail/mail/inbox"
	set postponed="/root/Mail/mail/postponed"
	set record="/root/Mail/mail/sent"

4.send mail:
	
	echo "邮件内容" |  /usr/bin/mutt  -s "邮件标题" "qq859755014@126.com"

