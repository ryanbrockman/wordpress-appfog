# wordpress-appfog
An example of running a basic WordPress site on the Appfog PaaS platform

# Basic installation and setup
1. Clone this repository
2. On Appfog, create a MySQL database service using CTL_MYSQL (this service will bind to your application later)
3. Edit the manifest.yml file
    a. Modify the application name to be something unique for your site
    b. Edit the services, modify wp_database to be the name of the new MySQL service you created in step 2 above
4. `cf push` your application to Appfog
5. After a successful push, your application should be reachable at the URL provided back by Appfog
    

# Utilities provided
1.  /phpinfo.php - Run this script to get a dump of the PHP configuration for your site.  This is helpful for verifying various PHP settings, extensions, etc.
2.  /opcache.php - By default, Zend Opcache extension is enabled for increased performance.  This utility script allows you to inspect the state of the Opcache.
    

# [Zend Opcache](http://us2.php.net/opcache)
Opcache is enabled by default. To disable it, edit the `/.bp-config/php/php.ini` file, setting `opcache.enable=0`.
    
# [Nginx FAST CGI cache](http://nginx.org/en/docs/http/ngx_http_fastcgi_module.html#fastcgi_cache)
FAST CGI cache in Nginx is enabled by default for better performance. 

A custom HTTP header has been included, called "X-Cache", that indicates cache HIT and MISS to check whether items are being cached successfully.

To disable the cache, edit the `/.bp-config/nginx/wordpress-single.conf` file.  Comment out line 25 so it looks like this: `#include fastcgi.conf;`

NOTE:  If you choose to leave FAST CGI cache enabled, you should be aware that the `fastcgi_cache_purge` directive is not currently supported. This is because the default version of Nginx that comes with the Cloud Foundry PHP buildpack does not currently include the fastcgi_cache_purge module. If/when this module is added, you can enable purging by uncommenting lines 29-31 in `/.bp-config/nginx/wordpress-single.conf`