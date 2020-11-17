---
title: Webserver setup with XAMMP for Mac
date: 2013-07-19
---

_Updated 2013-12-11_

I use XAMPP as a web server for the time being. I wrote down the installation process when I configured it on my new computer.

It addresses som issues I had when installing XAMMP, and also explains how to store your websites inside a Dropbox folder, instead of the default `htdocs/`.

> The directories written down, such as `/Applications/XAMPP/xamppfiles/` can of course differ, and you have to look up where your files are installed.

## Step by step

1. [Download XAMPP](http://www.apachefriends.org/en/xampp-macosx.html)
2. Install XAMPP (Instructions on the website)
3. Make the htdocs point to your Dropbox folder _(Optional)_
   - `cd /Applications/XAMPP/xamppfiles/`
   - Change name of the default htdocs folder, e.g htdocs-old. `mv htdocs/ htdocs-old/`
   - Create a symbolic link named htdocs, pointing to where you want to have your Sites. `ln -s ~/Dropbox/Sites/ htdocs`
4. Restart the web server.
5. Visit `http://localhost`
   - If you get 403, you have to change User/Group in `httpd.conf`, located in `/Applications/XAMPP/xamppfiles/etc/`. Change these to your username, and probably staff as group
6. Fix the "Session token" problem when trying to access phpmyadmin
   - Open `/Applications/XAMPP/xamppfiles/etc/php.ini`
   - Locate `session.save_path = "/tmp"` and uncomment that
7. Create virtual hosts, and hosts
   - Include `etc/extra/httpd-vhosts.conf` in `httpd.conf`
   - Create a virtual host entry in `httpd-vhosts.conf` for the general localhost
   - Also add a DNS entry to `/private/etc/hosts` to map the ServerName to your local IP, 127.0.0.1
8. Enable Gzip Compression (Read instructions below)

## /private/etc/hosts

File: `/private/etc/hosts`

    # Setup localhost
    127.0.0.1	localhost

    # Own projects
    127.0.0.1 pudge.lcl
    127.0.0.1 wordpress.lcl

    # Clients
    127.0.0.1 clientname.lcl
    127.0.0.1 dev.anotherclient.lcl

## etc/extra/httpd-vhosts.conf

File: `/Applications/XAMPP/etc/extra/httpd-vhosts.conf`

### localhost entry

    <VirtualHost *:80>
    	ServerName localhost
    	DocumentRoot "/Users/victormeyer/Dropbox/Sites"
    	<Directory "/Users/victormeyer/Dropbox/Sites">
    		Options Indexes FollowSymLinks Includes execCGI
    		AllowOverride All
    		Order Allow,Deny
    		Allow From All
    	</Directory>
    </VirtualHost>

### custom host entry

Replace `yourwebsite` with whatever you name your website.

    <VirtualHost *:80>
    	ServerName yourwebsite.lcl
    	DocumentRoot "/Users/victormeyer/Dropbox/Sites/yourwebsite"
    	<Directory "/Users/victormeyer/Dropbox/Sites/yourwebsite">
    		Options Indexes FollowSymLinks Includes ExecCGI
    		AllowOverride All
    		Order Allow,Deny
    		Allow From All
    	</Directory>
    	ErrorLog "logs/yourwebsite.lcl-error_log"
    </VirtualHost>

## XAMPP/xamppfiles/etc/extra/httpd-xampp.conf

File: `/Applications/XAMPP/xamppfiles/etc/extra/httpd-xampp.conf`

    #
    # Enable gzip compression
    # <http://www.mydigitallife.info/configure-and-enable-gzip-compression-with-mod_deflate-to-speed-up-apache-and-save-bandwidth/
    #
    AddOutputFilterByType DEFLATE text/html
    AddOutputFilterByType DEFLATE text/plain
    AddOutputFilterByType DEFLATE text/xml
    AddOutputFilterByType DEFLATE text/css
    AddOutputFilterByType DEFLATE text/javascript
    AddOutputFilterByType DEFLATE application/javascript
    AddOutputFilterByType DEFLATE application/xhtml+xml
    AddOutputFilterByType DEFLATE application/xml
    AddOutputFilterByType DEFLATE application/rss+xml
    AddOutputFilterByType DEFLATE application/atom_xml
    AddOutputFilterByType DEFLATE application/x-javascript
    AddOutputFilterByType DEFLATE application/x-httpd-php
    AddOutputFilterByType DEFLATE application/x-httpd-fastphp
    AddOutputFilterByType DEFLATE application/x-httpd-eruby
    AddOutputFilterByType DEFLATE image/svg+xml

## MongoDB Support

Read <http://www.php.net/manual/en/mongo.installation.php#mongo.installation.osx>.
Basically download the drivers, and add mongo as an php extension, by including the following line to your `php.ini`.

    extension=mongo.so

## cURL issue - cacert.pem

When I was working with the Twitter API, it all of a sudden stopped working, not knowing why really, I found that it had something to do with cURL permissions. I read <https://github.com/J7mbo/twitter-api-php/issues/39> and <https://github.com/J7mbo/twitter-api-php/issues/21#issuecomment-23699461> who had the same problem. The solution was to download `cacert.pem` from <http://curl.haxx.se/docs/caextract.html> and reference it in your `php.ini`, like so:

    ; issue with cURL
    ; https://github.com/J7mbo/twitter-api-php/issues/21#issuecomment-23699461
    ; Added 2013-12-11
    curl.cainfo=/Users/victormeyer/bin/cacert.pem

## Extra tips and tricks

### Add shortcuts to vhosts, hosts and logs

Add the following lines to your `~/.bash_profile`, where slime is my text editor of choise; Sublime Text 2. After you've added these lines, restart the terminal and try them out by just typing the alias (e.g `hosts`) and hit Enter.

    # Sublime Text 2 alias
    alias slime='open -a "/Applications/Sublime Text 2.app"'

    # Shortcuts
    alias hosts="slime /private/etc/hosts;"
    alias vhosts="slime /Applications/XAMPP/etc/extra/httpd-vhosts.conf;"
    alias logs="slime /Applications/XAMPP/xamppfiles/logs";
