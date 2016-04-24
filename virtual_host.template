server
{
    server_name             www.DOMAIN;

    listen                  80;

    #listen                 443 ssl;
    #include                boilerplate/enable/ssl.conf;
    #ssl_certificate        /etc/nginx/certs/server.crt;
    #ssl_certificate_key    /etc/nginx/certs/server.key;

    include                 boilerplate/disable/logging.conf;

    return                  301 http://DOMAIN$request_uri;
}

server
{
    server_name             DOMAIN;

    root                    DIR;
    listen                  80;

    #listen                 443 ssl;
    #include                boilerplate/enable/ssl.conf;
    #ssl_certificate        /etc/nginx/certs/server.crt;
    #ssl_certificate_key    /etc/nginx/certs/server.key;

    include                 boilerplate/enable/uploads.conf;
    include                 boilerplate/enable/gzip.conf;
    include                 boilerplate/locations/system.conf;
    include                 boilerplate/locations/errors.conf;
    include                 boilerplate/limits/methods.conf;

    location ~ ^.+\.php(?:/.*)?$
    {
        include                 boilerplate/enable/php.conf;
        fastcgi_pass            PHPVERSION;
        limit_req               zone=reqPerSec10 burst=50 nodelay;
        limit_conn              conPerIp 10;
    }

    access_log              /var/log/nginx/DOMAIN.bots.log main if=$is_bot buffer=10k flush=1m;
    access_log              /var/log/nginx/DOMAIN.access.log main if=!$is_bot buffer=10k flush=1m;
    error_log               /var/log/nginx/DOMAIN.error.log error;

    limit_req               zone=reqPerSec20 burst=100 nodelay;
    limit_conn              conPerIp 20;

    include                 boilerplate/locations/main.conf;
    include                 boilerplate/locations/static.conf;
}
