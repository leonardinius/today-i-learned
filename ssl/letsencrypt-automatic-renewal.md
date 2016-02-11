# Let's encrypt automatic renewal procedure
Today I learned how to use [Let's encrypt certificate program][1] to
automatically and periodically renew HTTPS/SSL certificates at @Galeoconsultig.

I hear you loudly wondering "Why?". The reason is the Let's encrypt initiative
provides you free of charge certificates with limited lifespan. At the moment
of writing the certificates are valid for 3 months. To update them manually
would the pain; hence the automation ant this post.

## Installation
I would recommend to take a look into Let's encrypt 
[docker image][running-with-docker]; however in this particular case it's
`32bit` box, so no Docker support at this moment.

Proceed with Let's encrypt [official client installation procedure][letsenrypt-installation].

## Prepare Webserver (nginx) for the acme challenge
Part of Let's encrypt automatic renewal process is [Automatic Certificate Management Environment or ACME challenge][acme-challenge].

In course of our work we don't want to restart web-server just to be able to
serve ACME challenge; so we will use webroot method to serve particular
well-known verification locations from webserver directory.

If/when succeed - we will gracefully reload the server configuration with
updates SSL certificates.

```nginx
server {
  listen      80;     server_name imaginary-domain imaginary-domain.galeoconsulting.com;

  location /.well-known/acme-challenge {
    root /var/www/letsencrypt/;
  }

  location / {
    return 301 https://imaginary-domain.galeoconsulting.com;
  }

  access_log /var/log/nginx/imaginary-domain_access.log combined buffer=32k flush=5m;
  error_log /var/log/nginx/imaginary-domain_error.log warn;
}
```

**What's happened? What did you do?**

We instructed HTTP:80 website (which we use to perform `HTTP 301 Permanent
redirect` to HTTPS/SSL version) to treat a special and "wel-known"
`/.well-known/acme-challenge` location specially and to serve static content
from Let's encrypt managed webroot directory `/var/www/letsencrypt/`.

## Create automatic renewal script
**File:** `/var/lib/letsencrypt/renew-site.sh`

```bash
#!/bin/bash

set -e
set -u
set -o pipefail

/var/lib/letsencrypt/letsencrypt/letsencrypt-auto \
  --text \
  --agree-tos \
  --renew-by-default \
  --webroot  \
  --webroot-path /var/www/letsencrypt/ \
  -m 'hostmaster@galeoconsulting.com' \
  certonly -d "${1}"
```

**What does it do?**
- `--text` specifies we want the script to interact to us using text interface
  (not curse dialogs).
- `--agree-tos` we agree to
  [Let's Encrypt Subscriber Agreement][letsencrypt-subscriber-agreement].
  Please find the actual version at [https://letsencrypt.org/repository/](https://letsencrypt.org/repository/)
- `--renew-by-default` forces SSL cert renewal even if not expired.
- `--webroot` specifies we will use the Apache/Nginx webroot to host and serve
  Let's encrypt generated challenge.
- `--webroot-path` path for client to put generated challenge file to.
- `-m <<email>>` DNS administrator email.
- `certonly` specifies we only need the client to get the certificates; we will
  do the rest.
- `-d <domain> -d <another-domain>` domains to generate SSL certificates for.

Let's call it once, so we get our seed SSL certificates.

I would do it here and paste the result; unfortunately I reached the maximum
renewal quota (5 in 7 days or 7 in 5; something between those lines. 
Basically I just got `Error creating new cert :: Too many certificates already
issued for: galeoconsulting.com` error.

So, please try on your own and report your findings / misconfigurations if you
do happen to stumble on something.

Ok. It was easy.

## Let's encrypt it then (Pun intended)

```nginx
server {
  listen      443 ssl;
  server_name imaginary-domain imaginary-domain.galeoconsulting.com;

   ssl on;
   ssl_certificate /etc/letsencrypt/live/imaginary-domain.galeoconsulting.com/fullchain.pem;
   ssl_certificate_key /etc/letsencrypt/live/imaginary-domain.galeoconsulting.com/privkey.pem;
   ssl_session_timeout 5m;
   ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
   ssl_ciphers ECDHE-RSA-AES256-GCM-SHA384:ECDHE-RSA-AES128-GCM-SHA256:DHE-RSA-AES256-GCM-SHA384:ECDHE-RSA-AES256-SHA384:ECDHE-RSA-AES128-SHA256:ECDHE-RSA-AES256-SHA:ECDHE-RSA-AES128-SHA:DHE-RSA-AES256-SHA:DHE-RSA-AES128-SHA;

  ## I have that configured globally
  # ssl_session_cache shared:SSL:50m;
  # ssl_dhparam /etc/nginx/certs/imaginary-domain.galeoconsulting.com_ssl_dhparam.pem;
  ssl_prefer_server_ciphers on;
}
```

## Let's put it all together
The easiest way to perform the auto-renewal is to put it in cronjob (which also
will notify us with email if something went wrong)

```cron
7    2 _ _   root    cd /tmp/ && /var/lib/letsencrypt/renew-site.sh imaginary-domain.galeoconsulting.com && service nginx reload
```

Try it. Enjoy it. _Vuala._

<!-- References -->
[1]: https://letsencrypt.org/ "Let's encrypt certificate program"
[running-with-docker]: http://letsencrypt.readthedocs.org/en/latest/using.html#running-with-docker "Let's encrypt running with Docker"
[letsenrypt-installation]: https://github.com/letsencrypt/letsencrypt#installation "Let's encrypt official client installation guide"
[letsencrypt-subscriber-agreement]: https://letsencrypt.org/documents/LE-SA-v1.0.1-July-27-2015.pdf "Let’s Encrypt Subscriber Agreement"
[acme-challenge]: https://letsencrypt.github.io/acme-spec/ "Automatic Certificate Management Environment (ACME)"
