<h4><b><samp>How to Install and Configure Postfix ?</samp></b></h4>

(1) yum install epel-release -y<br>
vi /etc/hosts                                    192.168.122.183 test.example.com<br>
setenforce 0 <br>
(2) firewall-cmd --permanent --add-port=80/tcp<br>
(3) firewall-cmd --reload <br>
(4)  yum install postfix -y <br>                                                                                              
(5) vi /etc/postfix/main.cf<br>
myhostname = rishabh.example.com<br>
 Line 85 - Uncomment  - mydomain = example.com<br>
 LIne 101 - Uncomment -  myorigin = $mydomain<br>
 Line 115 - Uncomment - inet_interfaces = all<br>
 Line 121 - inet_protocols = all<br>
 Line 166 = Comment<br>
 Line 167 - mydestination = $myhostname, localhost.$mydomain, localhost, $mydomain<br>
 Line 266 -  mynetworks = 192.168.122.100/24, 127.0.0.0/8<br>
 Line 421 - home_mailbox = Maildir/<br>
(6) systemctl enable postfix<br>
(7) systemctl restart postfix<br>
 
 
 
 
telnet localhost 25<br>
ehlo yourdomain.com<br>
mail from: username@example.com<br>
rcpt to: friend@hotmail.com, friend2@yahoo.com<br>
data<br>
Subject: My Telnet Test Email<br><br>
 
Hello,<br>
 
This is an email sent by using the telnet command.<br>
 
If I want to exit in telent   Ctrl + ]   or telnet> close <br>







<h4><b><samp>How to install and configure Dovecot ?</samp></b></h4>


(1) yum install dovecot* -y 
 
(2) vi /etc/dovecot/conf.d/10-mail.conf 
mail_location = maildir:~/Maildir # Line 24 - uncomment
 
(3) [root@fosnix ~]# vi /etc/dovecot/dovecot.conf
protocols = imap pop3 lmtp # Line 24 - umcomment
 
 
(4) [root@fosnix ~]# vi /etc/dovecot/conf.d/10-auth.conf
disable_plaintext_auth = yes #line 10 - uncomment
auth_mechanisms = plain login #Line 100 - Add the word: "login"
 
(5) [root@fosnix ~]# vi /etc/dovecot/conf.d/10-master.conf
#Line 91, 92 - Uncomment and add "postfix"
#mode = 0600
user = postfix
group = postfix
 
 
 
(3) systemctl restart dovecot 
(4) systemctl enable dovecot 
(5) firewall-cmd --permanent --add-port=110/tcp
(6) firewall-cmd --permanent --add-port=143/tcp
(7) firewall-cmd --reload 

Please find the below link for Install and configuration of squirrelmail on centos 7.
 
https://www.fosnix.com/install-and-configure-postfix-dovecot-squirrelmail-on-centos-7/?i=1

Testing Dovecot 


telnet localhost pop3

user sangwan           # Enter the mail user name 

pass password          # Enter the password

retr 1                 # Type this command to view mail

quit                    # Type 'quit' to exit







<h4><b><samp>How to install and configure Squirrelmail ?</samp></b></h4>

Install Squirrelmail
yum -y install squirrelmail


# cd /usr/share/squirrelmail/config/

# ./conf.pl

Press 1 and Then I can change 
1. Organization Name :
2. Organization logo :
8. Provider name : 
Press Save S and R return to home and Press 2 Change 

1. Domain :
3. Sendmail or SMTP :

Press to SMTP 2

vi /etc/httpd/conf/httpd.conf

Alias /webmail /usr/share/squirrelmail
<Directory /usr/share/squirrelmail>
Options Indexes FollowSymLinks
RewriteEngine On
AllowOverride All
DirectoryIndex index.php
Order allow,deny
Allow from all
</Directory>

(8) systemctl restart httpd
(9) Now I'll see and Use the SquirrelMail with GUI 

http://192.168.122.60/webmail/src/redirect.php




<h4><b><samp>How to install and Configure change password plugin for squirrelmail using poppassd ? ?</samp></b></h4>

(1) - Download change password plugin from https://squirrelmail.org/plugin_view.php?id=21 ( https://squirrelmail.org/countdl.php?fileurl=http%3A%2F%2Fwww.squirrelmail.org%2Fplugins%2Fchange_pass-3.1-1.4.0.tar.gz )
(2) - Download compability plugin from https://squirrelmail.org/plugin_view.php?id=152 ( https://squirrelmail.org/countdl.php?fileurl=http%3A%2F%2Fwww.squirrelmail.org%2Fplugins%2Fcompatibility-2.0.16-1.0.tar.gz )
(3) - mv countdl.php\?fileurl\=http\:%2F%2Fwww.squirrelmail.org%2Fplugins%2Fchange_pass-3.1-1.4.0.tar.gz  plugin_change_pass-3.1-1.4.0.tar.gz
(4) - tar -xvf plugin_change_pass-3.1-1.4.0.tar.gz
(5) - mv countdl.php\?fileurl\=http\:%2F%2Fwww.squirrelmail.org%2Fplugins%2Fcompatibility-2.0.16-1.0.tar.gz plugin_compatibility-2.0.16-1.0.tar.gz
(6) - tar -xvf plugin_compatibility-2.0.16-1.0.tar.gz
(7) - mv change_pass /usr/share/squirrelmail/plugins/
(8) - mv compatibility compability_plugin
(9) - Download poppassd.c from https://netwinsite.com/poppassd/
      Look at poppassd.c and make sure it looks safe
(10) - yum -y install gcc
(11) - gcc poppassd.c -o poppassd -lcrypt
(12) - mv poppassd /usr/local/bin/
(13) - yum -y install xinetd
(14) - cp /etc/xinetd.d/time-stream /etc/xinetd.d/poppassd
(15) - vim /etc/xinetd.d/poppassd
       Update "service time" to "service poppassd"
       disable = no
       id = poppasswd
       type = UNLISTED
       user = root
       group = root
       server = /usr/local/bin/poppassd
       port = 106
	


16. systemctl restart xinetd
17. systemctl enable xinetd
18. Test by doing "telnet localhost 106" that service is started properly or not
19. cd /usr/share/squirrelmail/config
    ./conf.pl
    Plugins
    compatilibility
    change_pass
    S
    Q
19. cd /usr/share/squirrelmail/plugins
20. vim change_pass/functions.php
 
Near line 54 comment "//if(!preg_match('/^2\d\d/', $result)) {" and insert another line "if(!preg_match('/Cha/', $result)) {"
without this change is not detected as due to some bug poppassd is not sending proper 200 response on CentOS 7.0
 

https://www.sbarjatiya.com/notes_wiki/index.php/CentOS_7.x_Configure_change_password_plugin_for_squirrelmail_using_poppassd


