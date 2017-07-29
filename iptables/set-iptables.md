# Basic Setup:

Here I have mentioned the basic configurations for enabling iptables in linux.

`iptables -L`

# Will list your current iptables configuration.

1) To allow established sessions to receive traffic

`iptables -A INPUT -m conntrack --ctstate ESTABLISHED,RELATED -j ACCEPT`

2) You could start by blocking traffic, but you might be working over SSH, where you would need to allow SSH before blocking everything else.

To allow incoming traffic on the default ssh port (22)

`iptables -A INPUT -p tcp --dport 22 -j ACCEPT`

To allow incoming traffic on the default Squid port (3128)

`iptables -A INPUT -p tcp --dport 3128 -j ACCEPT`

To allow incoming traffic on the default Apache port

`iptables -A INPUT -p tcp --dport 80 -j ACCEPT`

To allow incoming traffic on the default samba port

`iptables -A INPUT -p udp --dport 137 -j ACCEPT iptables -A INPUT -p udp --dport 138 -j ACCEPT iptables -A INPUT -p udp --dport 139 -j ACCEPT iptables -A INPUT -p tcp --dport 139 -j ACCEPT iptables -A INPUT -p tcp --dport 445 -j ACCEPT`

To allow incoming traffic on the default SNMP port (161)

`iptables -A INPUT -p tcp --dport 161 -j ACCEPT iptables -A INPUT -p udp --dport 161 -j ACCEPT`

Now check the current configuration

`iptables -L` `Chain INPUT (policy ACCEPT) target prot opt source destination ACCEPT all -- anywhere anywhere state RELATED,ESTABLISHED ACCEPT tcp -- anywhere anywhere tcp dpt:http ACCEPT tcp -- anywhere anywhere udp dpt:netbios-ns ACCEPT tcp -- anywhere anywhere udp dpt:netbios-dgm ACCEPT tcp -- anywhere anywhere udp dpt:netbios-ssn ACCEPT tcp -- anywhere anywhere tcp dpt:netbios-ssn ACCEPT tcp -- anywhere anywhere tcp dpt:snmp ACCEPT tcp -- anywhere anywhere udp dpt:snmp ACCEPT tcp -- anywhere anywhere tcp dpt:microsoft-ds ACCEPT tcp -- anywhere anywhere tcp dpt:squid`

3) Once we enabled the above port.we can drop all other incoming ports.

`iptables -A INPUT -j DROP`

Now check the rule

`iptables -L`

For Interface based access for eth0 specify -i eth0

4) In the final step we have to enable loopback interface. After all the traffic has been dropped. We need to insert this rule before that. Since this is a lot of traffic, we'll insert it as the first rule so it's processed first.

`iptables -I INPUT 1 -i lo -j ACCEPT`

5) To enabling logging

`iptables -I INPUT 5 -m limit --limit 5/min -j LOG --log-prefix "iptables denied: " --log-level 7`

6) To save this configuration

`iptables-save > /etc/sysconfig/iptables` or `service iptables save` `service iptables start`

This configuration will enable ssh port and disable all other incoming ports. To manually edit iptables config Also you can manual edit /etc/sysconfig/iptables

# IP Tables configuration for other Services

1) Iptables for default ldap port

`iptables -A INPUT -p tcp --dport 389 -j ACCEPT` `iptables -A INPUT -p tcp --dport 636 -j ACCEPT`

or manually edit /etc/sysconfig/iptables and add the below mentioned line

`-A INPUT -p tcp -m tcp --dport 389 -j ACCEPT`

2) Iptables for Backup Exec

3) IP tables for smtp

`iptables -A INPUT -p tcp --dport 25 -j ACCEPT`

or manually edit /etc/sysconfig/iptables and add the below mentioned line

`-A INPUT -p tcp -m tcp --dport 25 -j ACCEPT`

4) iptables for smtps

`iptables -A INPUT -p tcp --dport 465 -j ACCEPT`

or manually edit /etc/sysconfig/iptables and add the below mentioned line

`-A INPUT -p tcp -m tcp --dport 465 -j ACCEPT`

5) iptables for pop3 , pop3s

`iptables -A INPUT -p tcp --dport 110 -j ACCEPT` `iptables -A INPUT -p tcp --dport 995 -j ACCEPT`

or manually edit /etc/sysconfig/iptables and add the below mentioned line

`-A INPUT -p tcp -m tcp --dport 110 -j ACCEPT` `-A INPUT -p tcp -m tcp --dport 995 -j ACCEPT`

6) iptables for imap , imaps

`iptables -A INPUT -p tcp --dport 143 -j ACCEPT` `iptables -A INPUT -p tcp --dport 993 -j ACCEPT`

or manually edit /etc/sysconfig/iptables and add the below mentioned line

`-A INPUT -p tcp -m tcp --dport 143 -j ACCEPT` `-A INPUT -p tcp -m tcp --dport 993 -j ACCEPT`

7) iptables for webmin default port

`iptables -A INPUT -p tcp --dport 10000 -j ACCEPT`

or manually edit /etc/sysconfig/iptables and add the below mentioned line

`-A INPUT -p tcp -m tcp --dport 1000 -j ACCEPT`

8) IPtables for named, domain

`iptables -A INPUT -p tcp --dport 53 -j ACCEPT` `iptables -A INPUT -p udp --dport 53 -j ACCEPT`

or manually edit /etc/sysconfig/iptables and add the below mentioned line

`-A INPUT -p tcp -m tcp --dport 53 -j ACCEPT` `-A INPUT -p udp -m udp --dport 53 -j ACCEPT`

9) iptables for TFTP server

`iptables -A INPUT -p udp --dport 69 -j ACCEPT`

or manually edit /etc/sysconfig/iptables and add the below mentioned line

`-A INPUT -p udp -m udp --dport 69 -j ACCEPT`

10) iptable configuration for DHCP server

`iptables -A INPUT -p udp --dport 67 -j ACCEPT` `iptables -A INPUT -p udp --dport 68 -j ACCEPT`

or manually edit /etc/sysconfig/iptables and add the below mentioned line

`-A INPUT -p udp -m udp --dport 67 -j ACCEPT` `-A INPUT -p udp -m udp --dport 68 -j ACCEPT`

11) iptables for NFS server- click here

12) iptables for FTP server - click here

13) iptables for NTP server

`iptables -A INPUT -p udp --dport 123 -j ACCEPT`

or manually edit /etc/sysconfig/iptables and add the below mentioned line

`-A INPUT -p udp -m udp --dport 123 -j ACCEPT`

# Credits

Thanks to <http://sandlininc.com/?p=339>
