# LinuxServer
## Secure linux server for hosting item catalog site
ip:52.58.37.72

ssh port : 2222

to browse the website propaply [because google sign-in does not accept ips as origins]

you have to edit your hosts file (found in /etc/hosts) and add the following line

`52.58.37.72     restaurants-Item-Catalog`

url:http://restaurants-item-catalog/

put the RSA private key found in the notes sent with submition on your computer in the following directory : 

~/.ssh/catalog [where catalog is a file]

Software installed:

1.apache2

2.sqlite3 (instead of postgreSQL) 

3.python3-pip

4.tree (to see the all directories)


