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
