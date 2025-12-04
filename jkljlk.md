a2enmod include
a2enmod authnz_ldap

htpasswd -b -c /var/www/htpasswd ada ada
htpasswd -b /var/www/htpasswd igor igor
cat /var/www/htpasswd
cp /var/www/htpasswd /etc/apache2/htpasswd


<Directory /var/www/>
    Options Includes Indexes FollowSymLinks
    AllowOverride None
    Require all granted
</Directory>

AddType text/html shtml
AddOutputFilter INCLUDES shtml

<VirtualHost *:80>
    ServerName asu1.asu.ia.pw.edu.pl
    DocumentRoot /var/www
    DirectoryIndex index.shtml
</VirtualHost>


<VirtualHost *:80>
    ServerName asu2.asu.ia.pw.edu.pl
    DocumentRoot /var/www
    DirectoryIndex index.shtml
    <Location />
        AuthUserFile /etc/apache2/htpasswd
        AuthName "HTPASSWD"
        AuthType Basic
        Require valid-user
    </Location>
</VirtualHost>


<VirtualHost *:80>
    ServerName asu3.asu.ia.pw.edu.pl
    DocumentRoot /var/www
    DirectoryIndex index.shtml
    <Location />
        AuthName "LDAP"
        AuthType Basic
        AuthBasicProvider ldap
        AuthLDAPURL ldap://localhost/dc=asu,dc=ia,dc=pw,dc=edu,dc=pl?cn?sub
        Require valid-user
    </Location>
</VirtualHost>


a2ensite asu1.asu.ia.pw.edu.pl.conf
a2ensite asu2.asu.ia.pw.edu.pl.conf
a2ensite asu3.asu.ia.pw.edu.pl.conf

service apache2 reload


htpasswd -b -c /var/www/htpasswd ada ada
htpasswd -b /var/www/htpasswd igor igor
cat /var/www/htpasswd


server {
    root /var/www;
    ssi on;
    index index.shtml;
    server_name asu4.asu.ia.pw.edu.pl;
    ssi_types text/shtml text/html;
}

server {
    root /var/www;
    ssi on;
    index index.shtml;
    server_name asu5.asu.ia.pw.edu.pl;

    auth_basic "Restricted Content";
    auth_basic_user_file /var/www/htpasswd;
}


nginx -t
nginx -s reload


<VirtualHost *:80>
    ServerName localhost

    <Location /asu1/>
        ProxyPass http://asu1.asu.ia.pw.edu.pl:80/
    </Location>
    
    <Location /asu2/>
        ProxyPass http://asu2.asu.ia.pw.edu.pl:80/
    </Location>
    
    <Location /asu3/>
        ProxyPass http://asu3.asu.ia.pw.edu.pl:80/
    </Location>

    <Location /asu4/>
        ProxyPass http://asu4.asu.ia.pw.edu.pl:80/
    </Location>

    <Location /asu5/>
        ProxyPass http://asu5.asu.ia.pw.edu.pl:80/
    </Location>
</VirtualHost>


service nginx stop

a2enmod proxy_http

service apache2 restart

rm /etc/nginx/sites-available/default


server {
    listen 81;
    server_name localhost;

    location /asu1/ {
        proxy_pass http://asu1.asu.ia.pw.edu.pl:80/;
    }
    location /asu2/ {
        proxy_pass http://asu2.asu.ia.pw.edu.pl:80/;
    }
    location /asu3/ {
        proxy_pass http://asu3.asu.ia.pw.edu.pl:80/;
    }
    location /asu4/ {
        proxy_pass http://asu4.asu.ia.pw.edu.pl:80/;
    }
    location /asu5/ {
        proxy_pass http://asu5.asu.ia.pw.edu.pl:80/;
    }
}



















...
define host{
    use                 generic-host
    host_name           localhost
    alias               localhost
    address             127.0.0.1
}
define host{
    use                 generic-host
    host_name           host1
    alias               host1
    address             192.168.1.10
}
define host{
    use                 generic-host
    host_name           host2
    alias               host2
    address             192.168.1.20
}
...
define service{
    use                     generic-service
    host_name               localhost
    service_description     Disk Space
    check_command           check_all_disks!20%!10%
}
define service{
    use                     generic-service
    host_name               host1
    service_description     Disk Space
    check_command           check_all_disks!20%!10%
}
define service{
    use                     generic-service
    host_name               host2
    service_description     Disk Space
    check_command           check_all_disks!20%!10%
}
...
define service{
    use                     generic-service
    host_name               localhost
    service_description     SSH
    check_command           check_ssh
}
define service{
    use                     generic-service
    host_name               host1
    service_description     SSH
    check_command           check_ssh
}
define service{
    use                     generic-service
    host_name               host2
    service_description     SSH
    check_command           check_ssh
}

