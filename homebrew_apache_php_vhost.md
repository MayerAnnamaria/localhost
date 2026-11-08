# macOS Local Web Server Setup

**Homebrew · Apache · PHP · Virtual Hosts · SSL · MySQL · phpMyAdmin**

> Setup notes for a local development environment. Commands, configuration values, paths, and original notes are preserved as provided; this document only improves their Markdown presentation.

## Table of Contents

1. [Installing Homebrew](#1-installing-homebrew)
2. [Installing Apache](#2-installing-apache)
3. [Apache Troubleshooting](#3-apache-troubleshooting)
4. [Additional Localhost Configuration](#4-additional-localhost-configuration)
5. [Installing PHP](#5-installing-php)
6. [Installing Self-Signed SSL](#6-installing-self-signed-ssl)
7. [Additional Virtual Host Configuration](#7-additional-virtual-host-configuration)
8. [Virtual Hosts](#8-virtual-hosts)
9. [Adding a New Virtual Host](#9-adding-a-new-virtual-host-mytesttest)
10. [Installing MySQL Server](#10-installing-mysql-server)

---

## 1. Installing Homebrew

**Check if Homebrew is installed and working.**

```bash
brew doctor
```

Expected output:

```text
Your system is ready to brew.
```

---

## 2. Installing Apache

**References:**
- [Video tutorial](https://www.youtube.com/watch?v=Tlu2Prm_UNw)
- [macOS Ventura: Apache and multiple PHP versions](https://getgrav.org/blog/macos-ventura-apache-multiple-php-versions)

### Step 1 — Stop default Apache

```bash
sudo apachectl stop
sudo launchctl unload -w /System/Library/LaunchDaemons/org.apache.httpd.plist 2>/dev/null
```

### Step 2 — Install Apache

```bash
brew install httpd
```

### Step 3 — Start Apache

```bash
brew services start httpd
```

### Step 4 — Check in the browser window

```text
http://localhost:8080
```

### Step 5 — Apache configuration

```bash
subl /opt/homebrew/etc/httpd/httpd.conf
```

Change:

```apache
Listen 8080
```

To:

```apache
Listen 80
```

### Step 6 — Change `DocumentRoot`

Search for the term `DocumentRoot`, and you should see the following line:

```apache
DocumentRoot "/opt/homebrew/var/www"
```

Change to:

```apache
DocumentRoot "/Users/your_user/Sites"
```

### Step 7 — Change the directory path

Also change it to:

```apache
<Directory "/Users/your_user/Sites">
```

### Step 8 — Update `AllowOverride`

In that same `<Directory>` block you will find an `AllowOverride` setting, this should be changed as follows:

From:

```apache
AllowOverride None
```

To:

```apache
AllowOverride All
```

### Step 9 — Uncomment the rewrite module

```apache
LoadModule rewrite_module lib/httpd/modules/mod_rewrite.so
```

### Step 10 — Change the user and group

```apache
User your_user
Group staff
```

### Step 11 — Change `ServerName`

From:

```apache
#ServerName www.example.com:8080
```

To:

```apache
ServerName localhost
```

### Step 12 — Create the `Sites` folder, if necessary

```bash
mkdir ~/Sites
echo "<h1>My User Web Root</h1>" > ~/Sites/index.html
```

### Step 13 — Restart Apache and check if all is OK

```bash
brew services stop httpd
brew services start httpd
```

### Step 14 — Check the URL

```text
http://localhost
```

---

## 3. Apache Troubleshooting

### 3.1 Check if Apache is running

If you get a message that the browser can't connect to the server, first check to ensure the server is up. You should see a few `httpd` processes if Apache is up and running.

```bash
ps -aef | grep httpd
```

### 3.2 Restart Apache

```bash
brew services restart httpd
```

### 3.3 Monitor the Apache error log

You can watch the Apache error log in a new Terminal tab/window during a restart to see if anything is invalid or causing a problem:

```bash
tail -f /opt/homebrew/var/log/httpd/error_log
```

**Meg lehet hasznalni:**

```bash
brew services stop httpd
brew services start httpd
brew services restart httpd
```

### 3.4 Check where Apache is installed

```bash
httpd -S
```

---

## 4. Additional Localhost Configuration

**Itt allitom be h hova mutasson a localhost**

```bash
sudo nano /etc/apache2/httpd.conf
```

```apache
DocumentRoot "/Library/WebServer/Documents"
<Directory "/Library/WebServer/Documents">
```

**Original note:** `change to:`

---

## 5. Installing PHP

**Original note:** `Installed httpd, php`

Reference: [macOS Ventura: Apache and multiple PHP versions](https://getgrav.org/blog/macos-ventura-apache-multiple-php-versions)

---

## 6. Installing Self-Signed SSL

Reference: [macOS Ventura: Apache SSL](https://getgrav.org/blog/macos-ventura-apache-ssl)

---

## 7. Additional Virtual Host Configuration

**Ebben a file-ban a kovetkezot adjuk hozza:**

```bash
sudo nano /private/etc/apache2/extra/httpd-vhosts.conf
```

**Add:**

```apache
<VirtualHost *:80>
        ServerAdmin mayer_annamaria@yahoo.com
        DocumentRoot "/Users/mayerannamaria/Sites/spssabccom01.test"
        ServerName spssabccom01.test
        ServerAlias www.spssabccom01.test
        ErrorLog "/private/var/log/apache2/spssabccom01.test-error_log"  
        CustomLog "/private/var/log/apache2/spssabccom01.test-access_log" common  

</VirtualHost>
```

---

## 8. Virtual Hosts

### Step 1 — Uncomment the module in `httpd.conf`

```bash
subl /opt/homebrew/etc/httpd/httpd.conf
```

```apache
LoadModule vhost_alias_module lib/httpd/modules/mod_vhost_alias.so
```

### Step 2 — Uncomment this include path

```apache
# Virtual hosts
Include /opt/homebrew/etc/httpd/extra/httpd-vhosts.conf
```

### Step 3 — Configure `httpd-vhosts.conf`

```bash
subl /opt/homebrew/etc/httpd/extra/httpd-vhosts.conf
```

This file has some instructions already but the important thing to remember is that these rules are matched in order.

When you set up virtual hosts, you will lose your older document root, so you will need to add back support for that first as a virtual host.

```apache
<VirtualHost *:80>
    DocumentRoot "/Users/mayerannamaria/Sites"
    ServerName localhost
</VirtualHost>

<VirtualHost *:443>
    DocumentRoot "/Users/mayerannamaria/Sites/spssabcom"
    ServerName spssabcom.test
    SSLEngine on
    SSLCertificateFile "/opt/homebrew/etc/httpd/certs/spssabcom.test.pem"
    SSLCertificateKeyFile "/opt/homebrew/etc/httpd/certs/spssabcom.test-key.pem"
</VirtualHost>

<VirtualHost *:443>
    DocumentRoot "/Users/mayerannamaria/Sites/spssabchu"
    ServerName spssabchu.test
    SSLEngine on
    SSLCertificateFile "/opt/homebrew/etc/httpd/certs/spssabchu.test.pem"
    SSLCertificateKeyFile "/opt/homebrew/etc/httpd/certs/spssabchu.test-key.pem"
</VirtualHost>
```

### Virtual Host Directory Warning

As you set up your `.test` virtual hosts, you may receive a warning such as:

```text
Warning: DocumentRoot [/Users/your_user/Sites/grav-admin] does not exist
```

when restarting Apache.

This just lets you know that the source directory listed for your virtual hosts is not present on the drive. It's an issue that can be resolved by editing this file with the corrected `DocumentRoot`.

---

## 9. Adding a New Virtual Host: `mytest.test`

### Step 1 — Create the local project directory

In `Sites` folder create a folder named: `mytest`, create an `index.php` file in this folder, otherwise it will give a forbidden problem.

### Step 2 — Add the URL to `/etc/hosts`

```bash
sudo nano /etc/hosts
```

```text
127.0.0.1 localhost spssabchu.test spssabcom.test mytest.test
::1 localhost
fe80::1%lo0 localhost
```

### Step 3 — Go to the certificates directory

```bash
cd /opt/homebrew/etc/httpd/certs
```

### Step 4 — Create the SSL certificate

```bash
mkcert mytest.test
```

### Step 5 — Add the virtual host configuration

```bash
sudo nano /opt/homebrew/etc/httpd/extra/httpd-vhosts.conf
```

```apache
<VirtualHost *:443>
    DocumentRoot "/Users/mayerannamaria/Sites/mytest"
    ServerName mytest.test
    <Directory "/Users/mayerannamaria/Sites/mytest">
        Options FollowSymLinks
        AllowOverride All
        Order allow,deny
        allow from all
    </Directory>
    SSLEngine on
    SSLCertificateFile "/opt/homebrew/etc/httpd/certs/mytest.test.pem"
    SSLCertificateKeyFile "/opt/homebrew/etc/httpd/certs/mytest.test-key.pem"
</VirtualHost>
```

### Step 6 — Check if Apache syntax is OK

```bash
apachectl configtest
```

### Step 6 — Restart Apache

```bash
brew services stop httpd
brew services start httpd
```

---

## 10. Installing MySQL Server

Reference: [How to install Apache, MySQL and phpMyAdmin on macOS Ventura](https://blog.devops.dev/how-to-install-apache-mysql-phpmyadmin-server-on-macos-ventura-c65e68387776)

### Step 1 — Download the MySQL installer

Visit [MySQL Downloads](https://dev.mysql.com/downloads/mysql/).

Select the **ARM 64-bit DMG Archive** and download the installer.

### Step 2 — Download phpMyAdmin

Visit [phpMyAdmin Downloads](https://www.phpmyadmin.net/downloads/) to download installer.
