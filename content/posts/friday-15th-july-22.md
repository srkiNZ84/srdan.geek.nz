+++ 
draft = false
date = 2022-07-14T16:48:56+12:00
title = "Setting up Mutual TSL with Prometheus"
description = "Setting up mutual TLS between Prometheus and Node Exporter"
slug = ""
authors = ["Srđan Đukić"]
tags = ["prometheus", "node-exporter", "tls", "ssl", "mTLS", "authentication"]
categories = []
externalLink = ""
series = []
+++
# Setting up Mutual TLS with Prometheus and Node Exporter

## About

So, recently I was tasked with trying to securely enable metrics collection from servers which did not have a
private/management IP, only a publicly routable IP address.

This was a challenge in that if we are going to expose metrics over the public internet, security becomes a much bigger
focus.

While it was tempting to rely on a single mechanism to restrict access (such as firewall rules to only allow connections
from the Prometheus server), experience has taught us that it's much better to not assume that any single mechanism will
work all the time (see: [Swiss Cheese Model](https://en.wikipedia.org/wiki/Swiss_cheese_model)). Instead we should adopt
several security mecahnisms such that if a single one fails, others will pick it up.

In that spirit, I started looking at other authentication and authorization mechanisms for Prometheus Node exporter and
came to the conclusion that client TLS certificates (also called mutual TLS) are probably the best way to go security 
wise (you get encryption to mitigate MITM attacks, along with authentication/authorization to prevent unauthorized access
 to the endpoint).

## How Prometheus mutual TLS works

At a high level, we want to achieve two things:

* Have the HTTP interface to the Prometheus server enable TLS
* Have the HTTP interface on our Node Exporters require TLS _and_ a valid client certificate (i.e. signed by our
  internal CA)

## Setting up TLS on the Prometheus server

The first step is to generate a CA and certificates for the Prometheus server and the Node exporter and sign them all. 
This is described in a previous blog post [here](https://srdan.geek.nz/posts/thursday-14th-july-22/). At the end of it,
we are left with three sets of files:

* ca-key.pem (CA private key)
* ca.pem (CA certificate, used for signing)
* prom.key (Prometheus private key)
* prom.pem (Prometheus certificate, signed by the CA) 
* prom-ne-key.pem (Node Exporter private key)
* prom-ne.pem (Node Exporter certificate, signed by the CA)

Next we need to modify the Prometheus server to use the web config file where our TLS configuration is going to go. On
Ubuntu 22.04, this means adding the --web-config argument to the /etc/default/prometheus file:

```
ARGS="--web.config.file=/etc/prometheus/web-config.yml"
```

The contents of the `web-config.yml` file are as follows:

```
tls_server_config:
  cert_file: prom.pem
  key_file: prom-key.pem
  client_ca_file: ca.pem
  client_auth_type: VerifyClientCertIfGiven
```

Once these changes have been made, we can restart the Prometheus server with "systemctl restart prometheus" which should
pick up the new configuration. We can verify it has started correctly with "systemctl status prometheus" or "journalctl
-f -u prometheus".

If it has started correctly, we can now verify that the TLS is working by checking that we are unable to access the 
Prometheus Web interface over plain HTTP, but HTTPS is working (i.e. http://localhost:9090/ gives an error 
but https://localhost:9090/ does not)

NOTE: In our example we are using a self signed CA that we generated, so the browser will likely complain about the
hostname not matching the certificate

## Setting up TLS on the Node exporter

For the Node exporter, we want to configure it to point to our web configuration file by modifying
`/etc/default/prometheus-node-exporter` to ensure it has the line:
```
ARGS="--web.config=/etc/prometheus/node-exporter-web-config.yml"
```

with the contents of the file being as follows:
```
tls_server_config:
  cert_file: prom-ne.pem
  key_file: prom-ne-key.pem
  client_ca_file: ca.pem
    #client_auth_type: RequestClientCert
  client_auth_type: RequireAndVerifyClientCert
```

We can now go ahead and restart the Node Exporter server and verify that it starts up correctly and is serving over TLS
the same as above.

Now we need to add Node Exporter to the scrape configs, specifying that it should pick up and use the client certificate
when connecting. In `/etc/prometheus/prometheus.yml` we add the following section (replace static configuration with
whatever is appropriate):
```
scrape_configs:
  - job_name: node
    # If prometheus-node-exporter is installed, grab stats about the local
    # machine by default.
    scheme: 'https'
    tls_config:
      ca_file: 'ca.pem'
      cert_file: 'prom.pem'
      key_file: 'prom-key.pem'
    static_configs:
      - targets: ['127.0.0.1:9100']
```

We can then verify whether this is working or not by navigating to the "Targets" section in the Prometheus server Web UI
(i.e. https://localhost:9090/classic/targets) and confirming that there are no errors scraping the Node Exporter
metrics.

## Further work

The above was a good setup, but needs to be tested with public facing IP's and hostname's. Especially it would be
interesting to see whether there is any checking of the client hostname when the certificate is presented.

## References
https://pkg.go.dev/crypto/tls

https://prometheus.io/docs/prometheus/latest/configuration/https/

https://prometheus.io/docs/prometheus/latest/configuration/configuration/#tls_config
