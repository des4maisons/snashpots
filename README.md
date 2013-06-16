snapshots
=========

the learn-to-become-a-dev workshop's imgur clone

Installing
====

``` bash
git clone git@github.2ndsiteinc.com:marguerite/snapshots.git
cd snapshots
curl -s https://getcomposer.org/installer | php
php composer.phar install
cp app/
cp .htaccess.example .htaccess
```

Point your browser at the app/ directory.

Running on Mountain Lion:
====

Mountain lion got rid of the ~/Sites directory so you'll need to
do a bit of monkeying around in order to get this up and running.
``` bash
mkdir ~/Sites
chown :_www ~/Sites
```

Then create (or reconfigure) the apache config file that
allows sites to be served from the ~/Sites directory:

``` bash
sudo vim /etc/apache2/users/$(whoami).conf
```

Edit the file to contain this:

``` config
<Directory "/Users/$your_username/Sites/">
    Options Indexes MultiViews +FollowSymLinks
    AllowOverride All
    Order allow,deny
    Allow from all
</Directory>
```

In particular, if the file already exists, these 2 configurations
should be updated:

``` config
    Options Indexes MultiViews +FollowSymLinks
    AllowOverride All
```

Restart apache:
``` bash
sudo apachectl restart
```

Follow the installation instructions above, cloning the snapshots
repo into your ~/Sites directory.

Point your browser at http://localhost/$your_username/snapshots/app
and you should see a "hello world" page with an awesome misspelling of snapshots.

You might need to uncomment the `RewriteBase` line in `app/.htaccess` if
http://localhost/$your_username/snapshots/app doesn't work but
http://localhost/$your_username/snapshots/app/bootstrap.php does.

Permissions
====

If you get forbidden errors or 500 errors, you might need to change the permissions
on some of the files. (The following assumes that the user under which apache is running
is `_www`, but that might not be the case, especially if you're trying this on linux.)

``` bash
cd ~/Sites/snapshots
chown :_www .
chmod g+r .
chown -R :_www logs cache app vendor
chmod g+wr logs cache
chmod g-r -R logs/README.md cache/README.md .git .gitignore README.md
```
