NAME
====

dnscrypt-wrapper - A server-side dnscrypt proxy.

DESCRIPTION
===========

This is dnscrypt wrapper (server-side dnscrypt proxy), which helps to add dnscrypt support to any name resolver.

This software is modified from
[dnscrypt-proxy](https://github.com/opendns/dnscrypt-proxy).

Only udp protocol is supported now, tcp is work in progress.

INSTALLATION
============

    $ git clone git://github.com/Cofyc/dnscrypt-wrapper.git
    $ make
    $ make install
    
Usage
=====

First, generate provider keypair:

    # stored in public.key/secret.key in current directory
    $ ./dnscrypt-wrapper --gen-provider-keypair

Second, generate crypt keypair:

    # stored in crypt_public.key/crypt_secret.key in current directory
    $ ./dnscrypt-wrapper --gen-crypt-keypair

Third, generate pre-signed certificate (use pre-generated key pairs):

    # stored in dnscrypt.cert in current directory
    $ /dnscrypt-wrapper --crypt-secretkey-file misc/crypt_secret.key --crypt-publickey-file=misc/crypt_public.key --provider-publickey-file=misc/public.key --provider-secretkey-file=misc/secret.key --gen-cert-file

Run the program with pre-signed certificate:

    $ ./dnscrypt-wrapper  -r 8.8.8.8:53 -a 0.0.0.0:54  --crypt-secretkey-file=misc/crypt_secret.key --crypt-publickey-file=misc/crypt_public.key --provider-cert-file=misc/dnscrypt.cert --provider-name=2.dnscrypt-cert.yechengfu.com -VV

Run dnscrypt-proxy to test againt it:

    # --provider-key is public key fingerprint in first step.
    $ ./dnscrypt-proxy -a 127.0.0.1:55 --provider-name=2.dnscrypt-cert.yechengfu.com -r 127.0.0.1:54 --provider-key=4298:5F65:C295:DFAE:2BFB:20AD:5C47:F565:78EB:2404:EF83:198C:85DB:68F1:3E33:E952
    $ dig -p 55 google.com @127.0.0.1

Optional, you can store genearted pre-signed certificate (binary string) in TXT record for your provider name, for example: 2.dnscrypt-cert.yourdomain.com. Then you can omit `--provider-cert-file` and `--provider-name` options.

Optional, add "-d/--daemonize" flag to run as daemon.

Run "./dnscrypt-wrapper -h" to view command line options.

See also
========
    
- http://dnscrypt.org/
- http://www.opendns.com/technology/dnscrypt/
