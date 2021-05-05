<h3><b><samp> (1) - How to Install and Configure Postfix ?</samp></b></h3>

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







<h3><b><samp> (2) - How to install and configure Dovecot ?</samp></b></h3>


(1) yum install dovecot* -y 
 
(2) vi /etc/dovecot/conf.d/10-mail.conf <br> 
mail_location = maildir:~/Maildir # Line 24 - uncomment
 
(3) [root@fosnix ~]# vi /etc/dovecot/dovecot.conf <br>
protocols = imap pop3 lmtp # Line 24 - umcomment
 
 
(4) [root@fosnix ~]# vi /etc/dovecot/conf.d/10-auth.conf <br>
disable_plaintext_auth = yes #line 10 - uncomment
auth_mechanisms = plain login #Line 100 - Add the word: "login"
 
(5) [root@fosnix ~]# vi /etc/dovecot/conf.d/10-master.conf <br>
#Line 91, 92 - Uncomment and add "postfix"
#mode = 0600
user = postfix
group = postfix
 
 
 
(6) systemctl restart dovecot <br>
(7) systemctl enable dovecot <br>
(8) firewall-cmd --permanent --add-port=110/tcp <br>
(9) firewall-cmd --permanent --add-port=143/tcp <br>
(10) firewall-cmd --reload <br>

Please find the below link for Install and configuration of squirrelmail on centos 7. <br>
 
https://www.fosnix.com/install-and-configure-postfix-dovecot-squirrelmail-on-centos-7/?i=1 <br>

Testing Dovecot 


telnet localhost pop3

user sangwan           # Enter the mail user name 

pass password          # Enter the password

retr 1                 # Type this command to view mail

quit                    # Type 'quit' to exit







<h3><b><samp> (3) - How to install and configure Squirrelmail ?</samp></b></h3>

Install Squirrelmail <br>
yum -y install squirrelmail


 cd /usr/share/squirrelmail/config/

./conf.pl

Press 1 and Then I can change 
1. Organization Name :
2. Organization logo :
8. Provider name : <br>
Press Save S and R return to home and Press 2 Change 

1. Domain :
3. Sendmail or SMTP :

Press to SMTP 2 <br>

vi /etc/httpd/conf/httpd.conf <br> 

Alias /webmail /usr/share/squirrelmail <br>
<Directory /usr/share/squirrelmail> <br>
Options Indexes FollowSymLinks  <br>
RewriteEngine On  <br>
AllowOverride All <br>
DirectoryIndex index.php <br>
Order allow,deny  <br>
Allow from all  <br>
</Directory>  <br>

(8) systemctl restart httpd  <br>
(9) Now I'll see and Use the SquirrelMail with GUI <br>

http://192.168.122.60/webmail/src/redirect.php <br>




<h3><b><samp> (4) - How to install and Configure change password plugin for squirrelmail using poppassd ? ?</samp></b></h3>

(1) - Download change password plugin from https://squirrelmail.org/plugin_view.php?id=21 ( https://squirrelmail.org/countdl.php?fileurl=http%3A%2F%2Fwww.squirrelmail.org%2Fplugins%2Fchange_pass-3.1-1.4.0.tar.gz ) <br>
(2) - Download compability plugin from https://squirrelmail.org/plugin_view.php?id=152 ( https://squirrelmail.org/countdl.php?fileurl=http%3A%2F%2Fwww.squirrelmail.org%2Fplugins%2Fcompatibility-2.0.16-1.0.tar.gz ) <br>
(3) - mv countdl.php\?fileurl\=http\:%2F%2Fwww.squirrelmail.org%2Fplugins%2Fchange_pass-3.1-1.4.0.tar.gz  plugin_change_pass-3.1-1.4.0.tar.gz <br>
(4) - tar -xvf plugin_change_pass-3.1-1.4.0.tar.gz <br>
(5) - mv countdl.php\?fileurl\=http\:%2F%2Fwww.squirrelmail.org%2Fplugins%2Fcompatibility-2.0.16-1.0.tar.gz plugin_compatibility-2.0.16-1.0.tar.gz <br>
(6) - tar -xvf plugin_compatibility-2.0.16-1.0.tar.gz <br>
(7) - mv change_pass /usr/share/squirrelmail/plugins/ <br>
(8) - mv compatibility compability_plugin <br>
(9) - Download poppassd.c from https://netwinsite.com/poppassd/ <br> 
      Look at poppassd.c and make sure it looks safe <br>
(10) - yum -y install gcc <br>
(11) - gcc poppassd.c -o poppassd -lcrypt <br>
(12) - mv poppassd /usr/local/bin/ <br>
(13) - yum -y install xinetd <br>
(14) - cp /etc/xinetd.d/time-stream /etc/xinetd.d/poppassd <br>
(15) - vim /etc/xinetd.d/poppassd <br>
       Update "service time" to "service poppassd" <br>
       disable = no <br>
       id = poppasswd <br> 
       type = UNLISTED <br> 
       user = root <br>
       group = root <br>
       server = /usr/local/bin/poppassd <br>
       port = 106 <br>
	
16. systemctl restart xinetd <br>
17. systemctl enable xinetd <br>
18. Test by doing "telnet localhost 106" that service is started properly or not <br>
19. cd /usr/share/squirrelmail/config <br>
    ./conf.pl <br>
    Plugins <br>
    compatilibility <br>
    change_pass <br> 
    S <br>
    Q <br>
19. cd /usr/share/squirrelmail/plugins <br>
20. vim change_pass/functions.php <br>
 
Near line 54 comment "//if(!preg_match('/^2\d\d/', $result)) {" and insert another line "if(!preg_match('/Cha/', $result)) {"
without this change is not detected as due to some bug poppassd is not sending proper 200 response on CentOS 7.0
 

https://www.sbarjatiya.com/notes_wiki/index.php/CentOS_7.x_Configure_change_password_plugin_for_squirrelmail_using_poppassd


<h3><b><samp> (5) - How to set Aliases ?</samp></b></h3>


(1) vim /etc/aliases <br>

(2) define in last line <br>

Person who should get root's mail <br>
root:          marc <br>
allemp          rishabh,aman,pravesh <br>

(3) postalias hash:/etc/aliases 

After defining the allemp name variable in the alias, we will only send the mail by selecting allemp, then all the user or mail id we define there 
will go to the mail.



<h3><b><samp> (6) - How to increase mail size to Send Big Mail ?</samp></b></h3>

(1) - vim /etc/php.ini <br>
post_max_size = 20M <br>
upload_max_filesize = 20M <br>
max_file_uploads = 20M <br>

(2) systemctl restart httpd <br>
(3) vim /etc/postfix/main.cf <br>
mailbox_size_limit = 50240000 <br>
message_size_limit = 20480000 <br>
(4) systemctl restart postfix <br>



<h3><b><samp> (7) - How to Add Address Book in SquirrelMail ?</samp></b></h3>

 
(1) cd /usr/share/squirrelmail/images/ <br>
(2) paste the railway png image and go to config file <br>
(3) - cd /usr/share/squirrelmail/config <br>
(4) ./conf.pl <br>
press 1 Organization Preferences <br>
And the press 2 Organization Logo <br>



<h3><b><samp> (8) - How to set Forwarding ?</samp></b></h3>

Ans - through forwarding, we will send mail to one user and that mail will also be sent to another user. If I put forwarding. <br>

(1) firstly i create go to /home/aman(user)/.forward <br>
(2) chown aman:aman /home/aman/.forward <br>
(3) then i can define some mail in .forward <br>




<h3><b><samp> (9) - How To Configure Postfix Relay in Centos 7 ?</samp></b></h3>

(1) - yum -y install Postfix mailx cyrus-sasl-plain <br>
(2) - systemctl start postfix <br>
(3) - systemctl restart postfix <br>
(4) - systemctl enable postfix <br>
(5) - vim /etc/postfix/main.cf <br>
 
relayhost = [smtp.gmail.com]:587 <br>
smtp_use_tls = yes <br>
smtp_sasl_auth_enable = yes <br>
smtp_sasl_password_maps = hash:/etc/postfix/sasl_passwd <br>
smtp_tls_CAfile = /etc/ssl/certs/ca-bundle.crt <br>
smtp_sasl_security_options = noanonymous <br>
smtp_sasl_tls_security_options = noanonymous <br>

(6) vim /etc/postfix/sasl_passwd <br>
[smtp.gmail.com]:587 username:password
(7) postmap /etc/postfix/sasl_passwd <br>
(8) chown root:postfix /etc/postfix/sasl_passwd* <br>
(9) chown 640 /etc/postfix/sasl_passwd* <br>
(10) systemctl reload postfix <br>
(11)  vim /etc/postfix/tls_policy <br>
[smtp.gmail.com]:587 encrypt
(7) echo ” this is a test mail.” | mail -s “test message” user@gmail.com <br>




<h3><b><samp> (10) -  How to Add Address Book in SquirrelMail ?</samp></b></h3>

(1) cd /usr/share/squirrelmail/ <br>
 mkdir data <br>
cd data/ <br>
(2) vim global_abook <br>
aman|aman||aman@test.example.com|Internet Ticketing <br>
(3) Systemctl restart httpd <br>
(4) cd /usr/share/squirrelmail/config <br>
(5) ./conf.pl <br>
(6) and then press 8 (Plugins) and set abook_import_export <br>
(7) Save and Exit



