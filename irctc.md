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


(1) yum install dovecot* -y <br>
 
(2) vi /etc/dovecot/conf.d/10-mail.conf <br>
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



