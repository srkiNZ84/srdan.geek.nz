+++ 
draft = false
date = 2022-07-14T12:08:24+12:00
title = "Generating CA and certificates with CFSSL"
description = "Talking about openssl replacement cfssl and how to use it to generate a CA and have it sign certificates"
slug = ""
authors = ["Srđan Đukić"]
tags = ["cfssh", "ssl", "ca", "cloudflare", "tls"]
categories = []
externalLink = ""
series = []
+++
# Generating a CA and signing certificates with CFSSL

## About

### Background
Recently I was trying to setup an example mutual TLS configuration between Prometheus (the server) and Node Exporter. In doing so I
used OpenSSL to generate a CA and certificates for Prometheus and then have the Node exporter trust any client
certificates signed by the CA.

It took a long time to generate the CA and sign the certs, mostly due to having to figure out the openssl command line
syntax and flags etc... eventually I got there, only to hit this issue:

[https://jfrog.com/knowledge-base/general-what-should-i-do-if-i-get-an-x509-certificate-relies-on-legacy-common-name-field-error/](https://jfrog.com/knowledge-base/general-what-should-i-do-if-i-get-an-x509-certificate-relies-on-legacy-common-name-field-error/)

So, it turns out that CN's are no longer trusted/recommended and all new certificates need to use SAN's 🤦

At this point, the thought of going through the same exercise with OpenSSL did not appeal to me, but I gave it a go,
only to hit another hurdle.

That's when I remembered that CloudFlare released an SSL library not too long ago with an explicit focus on security and
usability. This post talks about using that library ([CFSSL](https://blog.cloudflare.com/introducing-cfssl/)) to generate a CA and certificates.

## Generating a CA

The first thing that we need to do is to generate a CA. The first step in this process is to generate a certificate
signing request in the CFSSL JSON format like the below:

    $ cat ca-csr.json 
    {
        "hosts": [
            "example.com",
            "www.example.com",
            "https://www.example.com",
            "jdoe@example.com",
            "127.0.0.1"
        ],
        "key": {
            "algo": "rsa",
            "size": 2048
        },
        "names": [
            {
                "C":  "NZ",
                "L":  "Auckland",
                "O":  "",
                "OU": "WWW",
                "ST": "Auckland"
            }
        ]
    }

NOTE: As we are generating a self-signed Certificate Authority for verifying mutual TLS, the DNS names do not have to match up.

We can then run the command below in order to generate our CA:

    cfssl genkey -initca ca-csr.json | cfssljson -bare ca

In the above, we are generating a CA using the values from the JSON file and then piping it to the cfssljson to save it
to two output files (`ca.pem` and `ca-key.pem`).

## Signing our certificate using our CA

Now that we have a CA, we need to generate a certificate for our host and sign it using the CA we have created in the
previous step. We can do this by creating a new CSR file, which looks similar to our original one:

    $ cat prom-csr.json 
    {
        "hosts": [
            "prom-example.com",
            "www.prom-example.com",
            "https://www.prom-example.com",
            "prom@example.com",
            "127.0.0.1"
        ],
        "key": {
            "algo": "rsa",
            "size": 2048
        },
        "names": [
            {
                "C":  "NZ",
                "L":  "Auckland",
                "O":  "",
                "OU": "WWW",
                "ST": "Auckland"
            }
        ]
    }

and then use it to generate a key, a certificate signing request and create a signed certificate with:

    cfssl gencert -ca ca.pem -ca-key ca-key.pem prom-csr.json | cfssljson -bare prom

which will result in two new output files, `prom.pem` and `prom-key.pem`.

# Conclusion and further work

So, there we have it. So much simpler than OpenSSL.

In terms of future work, it would be good to investigate whether CFSSL has been vetted/analysed by any third parties
regarding its security properties. It is [open source](https://github.com/cloudflare/cfssl) and for
the most part relies on Go's standard libraries for the cryptographic functions, so _should_ be ok.

Other questions raised during this exercise are to do with revocation lists and certificates and how those are handled.

I've also discovered that you can actually run cfssl as a server using the "serve" command, which is also very useful.
For more reading about features and functionality, the [CFSSL tag](https://blog.cloudflare.com/tag/cfssl/) on the CloudFlare blog is a good start.
