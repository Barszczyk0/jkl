dn: ou=People,dc=asu,dc=ia,dc=pw,dc=edu,dc=pl
ou: People
objectClass: organizationalUnit

dn: ou=Groups,dc=asu,dc=ia,dc=pw,dc=edu,dc=pl
objectClass: organizationalUnit
ou: Groups

dn: cn=students,ou=Groups,dc=asu,dc=ia,dc=pw,dc=edu,dc=pl
objectClass: posixGroup
cn: students
gidNumber: 5000

dn: uid=jan,ou=People,dc=asu,dc=ia,dc=pw,dc=edu,dc=pl
uid: jan
cn: Jan Kowalski
# objectClass: inetOrgPerson ?
objectClass: account
objectClass: posixAccount
objectClass: top
objectClass: shadowAccount
userPassword: jan
loginShell: /bin/bash
uidNumber: 5000
gidNumber: 5000
homeDirectory: /home/users/jan
gecos: Jan Kowalski,,,


ldapadd -x -D cn=admin,dc=asu,dc=ia,dc=pw,dc=edu,dc=pl -W -f ~/jan.ldif
sudo auth-client-config -t nss -p lac_ldap
sudo pam-auth-update


sudo auth-client-config -t nss -p lac_ldap
sudo pam-auth-update









iptables -A FORWARD -i eth2 -o eth0 -m state --state RELATED,ESTABLISHED -j ACCEPT
iptables -A FORWARD -i eth0 -o eth2 -j ACCEPT
iptables -t nat -A POSTROUTING -j MASQUERADE

iptables -t nat -A PREROUTING -p tcp -i eth1 --dport 80 -j DNAT --to-destination 192.168.1.10:80
iptables -t nat -A PREROUTING -p tcp -i eth2 --dport 80 -j DNAT --to-destination 192.168.1.10:80

iptables -t nat -A PREROUTING -p tcp -i eth1 --dport 22 -j DNAT --to-destination 192.168.1.20:22
iptables -t nat -A PREROUTING -p tcp -i eth2 --dport 22 -j DNAT --to-destination 192.168.1.20:22

iptables -A INPUT -p udp --dport 53 -s 192.168.1.0/24 -j ACCEPT
iptables -A INPUT -p tcp --dport 53 -s 192.168.1.0/24 -j ACCEPT

iptables -A INPUT -p udp --dport 53 -j DROP
iptables -A INPUT -p tcp --dport 53 -j DROP

iptables-save > /etc/iptables/rules.v4
iptables-save 

