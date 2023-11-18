
## savatin/nginx-php

Run this [Nginx](http://nginx.org/) image with:

    $ docker run -d --name nginx-0 \
        -v /var/www/nginx-0/htdocs:/var/www/localhost \
        -p 80:80 -p 443:443 \
        savatin/nginx-php

Comes bundled with [php](http://php.org/) / [fpm](http://php-fpm.org/) / [xdebug](http://xdebug.org/) (disabled per default) / [composer](https://getcomposer.org/).
To enable xdebug, mapped to port 9000 on the host:

    $ docker run -d --name nginx-0 -v /var/www/nginx-0/htdocs:/var/www/localhost -p 80:80 -p 443:443 \
        -p 9000:9000 \
        -e XDEBUG_ENABLED=yes \
        savatin/nginx-php

The image also provides optional phpinfo.php and [adminer](http://www.adminer.org/en/) micro sites. To enable them:

    $ docker run -d --name nginx-0 -v /var/www/nginx-0/htdocs:/var/www/localhost -p 80:80 -p 443:443 \
        -e NG_TMPL_ADMINER_URL=db.test.void \
        -e NG_TMPL_PHPINFO_URL=phpinfo.test.void \
        savatin/nginx-php

This will result in 2 vhost config files for:

    http://db.test.void
    http://phpinfo.test.void

Your dns must resolve {db,phpinfo}.test.void to the localhost for this to work, which can be painful if you have many different sites.
It's much easier to have the nginx-proxy image running and resolve *.void with something like dnsmasq to your localhost instead:

    $ docker run -d --name nginx-0 -v /var/www/nginx-0/htdocs:/var/www/localhost \
        -e VIRTUAL_HOST=test.void,db.test.void,phpinfo.test.void \
        -e NG_TMPL_ADMINER_URL=db.test.void \
        -e NG_TMPL_PHPINFO_URL=phpinfo.test.void \
        savatin/nginx-php

Notice the missing -p flags, as the nginx-proxy is bound to port 80 and 443 on the host already. The VIRTUAL_HOST env will be
picked up by the nginx-proxy container and it will route all requests accordingly.

---

![PACKAGES](PACKAGES.md)