# TLS View

Tool to verify if the TLS connection parameters and the certificates usage on the target server is using good practices.

```
❯ ./src/tlsscan.sh --csv /tmp/out.csv -f addr.list; csvlook /tmp/out.csv
checking [google.com:443 Google]...
checking [accuknox.com:443 Accuknox]...
checking [expired.badssl.com:443 BadSSL]...
checking [wrong.host.badssl.com:443 BadSSL]...
checking [self-signed.badssl.com:443 BadSSL]...
checking [untrusted-root.badssl.com:443 BadSSL]...
checking [revoked.badssl.com:443 BadSSL]...
checking [pinning-test.badssl.com:443 BadSSL]...
checking [dh480.badssl.com:443 BadSSL]...
checking [isunknownaddress.com:12345 LocalTest]...
checking [localhost:1234]...
checking [localhost:22 namespace:deployment/wordpress]...
| Name                           | Address                       | Status   | Version | Ciphersuite                 | Hash   | Signature | Verification                                 |
| ------------------------------ | ----------------------------- | -------- | ------- | --------------------------- | ------ | --------- | -------------------------------------------- |
| Google                         | google.com:443                | TLS      | TLSv1.3 | TLS_AES_256_GCM_SHA384      | SHA256 | ECDSA     | OK                                           |
| Accuknox                       | accuknox.com:443              | TLS      | TLSv1.3 | TLS_AES_256_GCM_SHA384      | SHA256 | RSA-PSS   | OK                                           |
| BadSSL                         | expired.badssl.com:443        | TLS      | TLSv1.2 | ECDHE-RSA-AES128-GCM-SHA256 | SHA512 | RSA       | certificate has expired                      |
| BadSSL                         | wrong.host.badssl.com:443     | TLS      | TLSv1.2 | ECDHE-RSA-AES128-GCM-SHA256 | SHA512 | RSA       | OK                                           |
| BadSSL                         | self-signed.badssl.com:443    | TLS      | TLSv1.2 | ECDHE-RSA-AES128-GCM-SHA256 | SHA512 | RSA       | self-signed certificate                      |
| BadSSL                         | untrusted-root.badssl.com:443 | TLS      | TLSv1.2 | ECDHE-RSA-AES128-GCM-SHA256 | SHA512 | RSA       | self-signed certificate in certificate chain |
| BadSSL                         | revoked.badssl.com:443        | TLS      | TLSv1.2 | ECDHE-RSA-AES128-GCM-SHA256 | SHA512 | RSA       | certificate has expired                      |
| BadSSL                         | pinning-test.badssl.com:443   | TLS      | TLSv1.2 | ECDHE-RSA-AES128-GCM-SHA256 | SHA512 | RSA       | OK                                           |
| BadSSL                         | dh480.badssl.com:443          | CONNFAIL |         |                             |        |           |                                              |
| LocalTest                      | isunknownaddress.com:12345    | CONNFAIL |         |                             |        |           |                                              |
| localhost:1234                 | localhost:1234                | CONNFAIL |         |                             |        |           |                                              |
| namespace:deployment/wordpress | localhost:22                  | CONNFAIL |         |                             |        |           |                                              |
```

## Docker Run

```
docker run --rm -v $PWD:/home/kubetls/data nyrahul/tlsscan --infile data/addr.list --csv data/out.csv
```
> Note: The command assumes that the current folder contains `addr.list` file containing the list of addresses to scan.
