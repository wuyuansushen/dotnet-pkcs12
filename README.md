# dotnet-pkcs12

Configure HTTPS of ASP.NET Core with TLS certificate.

## 1. Examine Self-signed Certificate Location

`~/.dotnet/corefx/cryptography/x509stores/my/[0-9A-F]{40}.pfx`

## 2.Export pem certificate

```
openssl pkcs12 -in ~/.dotnet/corefx/cryptography/x509stores/my/[0-9A-F]{40}.pfx -out cert.pem -nokeys -nodes
```

## 3.Add to CA Store

```
sudo cp cert.pem /etc/pki/ca-trust/source/anchors/
sudo update-ca-trust
```

## 4.Examine certificates

```
trust list --filter certificates | grep -i -n localhost
```

 :warning: Examine whether output *localhost* information after running preceding code

> pkcs11:id=%FF%FF%FF%FF%FF%FF%FF%FF%FF%FF%FF%FF%FF%FF%FF%FF%FF%FF%FF%FF;type=cert
    type: certificate
    label: localhost
    trust: anchor
    category: other-entry

## 5.Use curl with self-signed certificate

> :warning: Remember to `unset` all proxy env various like *all_proxy* or add `localhost` to *no_proxy*

Connect *localhost* express server without CA certificate file:

> curl: (60) SSL certificate problem: self signed certificate
More details here: https://curl.haxx.se/docs/sslcerts.html
curl failed to verify the legitimacy of the server and therefore could not
establish a secure connection to it. To learn more about this situation and
how to fix it, please visit the web page mentioned above.

