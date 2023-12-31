## caddy_config_examples
## caddy did not show index file on the specified paths here some examples how to configure caddy


i thsted this on caddy v2.7.5

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



Now i made a new folder /etc/caddy/conf.d and create there the files domain1.conf and domain2.conf i will only show a example of domain1.conf

```bash
# filename: /etc/caddy/conf.d/domain1.conf
#
# My example configuration for the domain1.tld

# Create LOGGIG for this domain
log {
       level DEBUG
       output file /var/log/caddy/domain1.log {
           roll_size 10MiB
           roll_keep 10
           roll_keep_for 336h
       }
    }


# Optional assing certificates by tls
tls /etc/ssl/certs/domain1/fullchain.pem /etc/ssl/certs/domain1/key.pem

# Path to my css
	handle_path /css/* {
		root *  /var/www/html/css
		file_server {
			index index.html
			browse
			}
}

 # A proxied uri in this case to localhost port 8080 but could be anything
       handle_path  /proxy* {
                reverse_proxy  127.0.0.1:8080
        }




# In my case i created a seperate handle for the domain root 
 handle / {
	import /etc/caddy/conf.d/domain1_root.conf
}

```

For the root domain i used a seperate file, in this file i createde a respond with some html included. Using a inline respond will serve mutch faster. In my case caddy works almost 60% faster using direct respond.
```bash
# file:  /etc/caddy/conf.d/domain1_root.conf

vars {
	title \"Site Name\"
	css " <link rel=\"stylesheet\"  <type=\"text/css\"   href=\"https://tlnd.nl/css/tables.css\" />"
	css-terminal " <link rel=\"stylesheet\"  <type=\"text/css\"   href=\"https://tlnd.nl/css/terminal.css\" />"
	}


header Content-Type text/html
respond "
  <!DOCTYPE html>
  <html lang=\"en\">
  <head>
  <title>{vars.title}</title>
  {vars.css-terminal}
  </head>
  <body>
	  {vars.body}
  </body>
  </html>

"


```
As you can see you need escape characters with double quotes if you put your static content to respond. You can use variables. In this case i made them in the same file, but you can also create a more global variable file and add it to your Caddy file or your domain file. 



