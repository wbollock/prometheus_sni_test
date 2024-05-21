# prometheus_sni_test

To test SNI capability of prometheus tls server_name config in light of this [github issue](https://github.com/prometheus/prometheus/issues/10868).

I may have a need to generate 1 global certificate for clients to present to Prometheus for scraping, and want to see if I can plug in any string into Prometheus' tls_config `server_name`, have it match the server `CN`, and get a successful scrape.

## Results

```yaml
# prometheus_config
    tls_config:
      ca_file: /certs/ca.crt
      cert_file: /certs/client.crt
      key_file: /certs/client.key
      server_name: service_name_test
---
# *server* certificate information
        Signature Algorithm: sha256WithRSAEncryption
        Issuer: CN = Test CA
        Validity
            Not Before: May 21 16:42:05 2024 GMT
            Not After : Oct  3 16:42:05 2025 GMT
        Subject: CN = service_name_test
```

Nginx logs show success even on TLS v1.3:

```console
nginx-1       | 192.168.16.3 - - [21/May/2024:16:53:23 +0000] "GET /metrics HTTP/1.1" 200 7 "-" "Prometheus/2.52.0" "nginx:8080" "service_name_test" "TLSv1.3"
```

## Usage

```shell
cd certs
make
cd ..
sudo docker compose up --build

# test and reset
cd certs
make clean
make
```
