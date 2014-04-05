Running apache instances
========================

Distributions: precise,saucy,wheezy

In some (really rare) case you may need to install an apache instance (heavily .htaccess based instances, mod_svn ...)

Installation
============

```sh
cp -R /etc/apache2 $HOME/etc
mkdir /var/log/apache2
```

Configuration
=============

Edit $HOME/etc/apache2/ports.conf to bind on unprivileged ports (like 8080)

Edit $HOME/etc/apache2/sites-enabled/000-default to set your DocumentRoot

Running it
==========

Use this vassal as your base configuration

```ini
[uwsgi]
; register the domains you need
domain = example.com
domain = example2.com
domain = .foo.bar

; load the http proxy router
plugins = router_http
offload-threads = 2
; forward requests to apache
route-run = http:127.0.0.1:8080
; monitor the apache instance
smart-attach-daemon = /run/apache2.pid apache2ctl -k start -d $(HOME)/etc/apache2
```


Common pitfalls
===============

Your configurations like in $HOME/etc/apache2 NOT /etc/apache2 !!! (if you get permission errors, very probably you are in the wrong dir)

