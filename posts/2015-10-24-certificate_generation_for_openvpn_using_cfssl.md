title: Certificate Generation for OpenVPN using CFSSL
published_date: "2015-10-24 20:43:01 +1100"
layout: default.liquid
data:
  layout: post
  comments: true
---
In the past when I have needed certificates for OpenVPN I have used easy\_rsa.
Another option would be to use OpenSSL directly. This time I decided to try
CFSSL. [CFSSL][cfssl] is a PKI toolkit from [CloudFlare][cf] that can function as an
API or a CLI tool. This blog post covers setting up a CA and generating
certificates and keys suitable for use with OpenVPN. It doesn't cover setting up
OpenVPN. For that, the [official documentation][ovpn] is handy.

CFSSL is written in Go and can be installed by following the instructions in the
[README][cfssl-git] file.

# CA Generation

Create a file, `ca-csr.json`, similar to the following. You will want to name
your CA something other than "Example Certificate Authority".

    {
        "CN": "Example Certificate Authority",
        "hosts": [
        ],
        "key": {
            "algo": "rsa",
            "size": 4096
        },
        "names": [
            {
                "C": "AU",
                "ST": "NSW",
                "L": "Sydney",
                "O": "Example",
                "OU": "Example Certificate Authority"
            }
        ]
    }

To generate our self-signed CA cert and key:

    $ cfssl genkey -initca ca-csr.json | cfssljson -bare ca
    2015/10/18 16:52:49 [INFO] generate received request
    2015/10/18 16:52:49 [INFO] received CSR
    2015/10/18 16:52:49 [INFO] generating key: rsa-4096
    2015/10/18 16:52:52 [INFO] encoded CSR
    2015/10/18 16:52:53 [INFO] signed certificate with serial number <SERIAL_NUMBER>

The `cfssl` command outputs JSON. To generate pem files we use the `cfssljson`
utility. The `-bare` flag is used as the JSON blob output by `cfssl` is missing
the API wrapping. We will now have files:

    $ ls  ca*
    ca.csr  ca-csr.json  ca-key.pem  ca.pem

Our CA certificate and key are in the `ca.pem` and `ca-key.pem` files
respectively.

**Note: the key file is not password protected so keep these files secure.**

# Client Certificates

With a CA generated we can now generate a certificate/key for each of the
OpenVPN clients. For each client/server we generate a CSR.

Once again the CSR is a JSON blob:

    {
        "CN": "client1.example.com",
        "hosts": [
            "client1.example.com"
        ],
        "key": {
            "algo": "rsa",
            "size": 4096
        },
        "names": [
            {
                "C": "AU",
                "ST": "NSW",
                "L": "Sydney",
                "O": "Example",
                "OU": "VPN"
            }
        ]
    }

To generate a certificate/key from this CSR signed by our CA:

    $ cfssl gencert -ca ca.pem -ca-key ca-key.pem client1.example.com-csr.json | cfssljson -bare client1.example.com
    2015/10/24 21:42:34 [INFO] generate received request
    2015/10/24 21:42:34 [INFO] received CSR
    2015/10/24 21:42:34 [INFO] generating key: rsa-4096
    2015/10/24 21:43:03 [INFO] encoded CSR
    2015/10/24 21:43:04 [INFO] signed certificate with serial number <SERIAL_NUMBER>

We will now have the following files:

    $ ls client1.example.com*
    client1.exmaple.com.csr  client1.exmaple.com-csr.json  client1.exmaple.com-key.pem  client1.exmaple.com.pem

Once again the certificate and key are in the `*.pem` and `*-key.pem` files.
Copy these files, along with the `ca.pem` file, via a secure method, to your VPN
client.

As mentioned above, CFSSL can run as an API so there is room to automate this
process if you are generating large numbers of certificates. I found CFSSL to
be fairly simple to use and I prefer it to easy\_rsa or using OpenSSL directly.

[cf]: https://www.cloudflare.com/
[ovpn]: https://openvpn.net/index.php/open-source/documentation/howto.html
[cfssl]: https://blog.cloudflare.com/introducing-cfssl/
[cfssl-git]: https://github.com/cloudflare/cfssl
