<h4><b><samp>How to Install and Configure Postfix ?</samp></b></h4>

(1) yum install epel-release -y
vi /etc/hosts                                    192.168.122.183 test.example.com
setenforce 0 
(2) firewall-cmd --permanent --add-port=80/tcp
(3) firewall-cmd --reload 
(4)  yum install postfix -y                                                                                               
(5) vi /etc/postfix/main.cf
myhostname = rishabh.example.com
# Line 85 - Uncomment  - mydomain = example.com
# LIne 101 - Uncomment -  myorigin = $mydomain
# Line 115 - Uncomment - inet_interfaces = all
# Line 121 - inet_protocols = all
# Line 166 = Comment
# Line 167 - mydestination = $myhostname, localhost.$mydomain, localhost, $mydomain
# Line 266 -  mynetworks = 192.168.122.100/24, 127.0.0.0/8
# Line 421 - home_mailbox = Maildir/
(6) systemctl enable postfix
(7) systemctl restart postfix
 
 
 
 
telnet localhost 25
ehlo yourdomain.com
mail from: username@example.com
rcpt to: friend@hotmail.com, friend2@yahoo.com
data
Subject: My Telnet Test Email
 
Hello,
 
This is an email sent by using the telnet command.
 
If I want to exit in telent   Ctrl + ]   or telnet> close


