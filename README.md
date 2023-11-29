# caddy_config_examples
caddy did not show index file on the specified paths here some examples how to configure caddy


In my main caddy file i made imports by domain. 

so my main caddy file looks like this

```bash
# filename: /etc/caddy/Caddyfile
#
# A simple example how i configure caddy by domain

# Here i link my the first domain to a seperate file
domain1.tld {
import /etc/caddy/conf.d/domain1.conf
}

# Here i link my the second domain to a seperate file
domain2.tld {
import /etc/caddy/conf.d/domain2.conf
}

```
