# easy apache2 maintenance wall
This let you configure an easy maintenance wall on your vhost to quickly serv a "maintenance info".

## How it works
In apache2 we can use `RewriteEngine` [^1] to rewrite requested URLs on the fly.
We redirect all requested URLs to a subpath "/maintenance" if an apache administrator enables the
"maintenance-function".
The maintenance website redirects every 10 seconds (default) to the url origin, for example:
`example.com/maintenance` automatically redirects to `example.com`
After disabling the "maintenance-function" the user will automatically redirected to the url
origin.

## Configuration
 * Copy the files in `src/` to `/var/www/maintenance` or `/var/www/maintenance/perVhostMaintenance`.
 * Copy the files in `config/` to `/etc/apache2/`.

### maintenance.conf 

If your files located at `/var/www/maintenance/perVhostGdpr` or something you have to redefine the
location in `maintenance.conf`:

    Alias /maintenance /var/www/maintenance -> Alias /maintenance /var/www/maintenance/perVhostGdpr

### your vhost
Here is a snippet for an example vhost

```
<--- snip --->

  ########### MAINTENANCE WALL #################
  Include maintenance.conf
  ########### MAINTENANCE WALL #################

  Alias /website /var/www/html/htdocs/website
  <Location /website>
    Require all granted
    Options +MultiViews
  </Location>

<--- snip --->

</VirtualHost>
</IfModule>

```

### index.php
You can find the php code for the url origin redirect here.

And don't forget to change the GDPR-text in the `index.html` file :)

[^1]: https://httpd.apache.org/docs/current/mod/mod_rewrite.html
