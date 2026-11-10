# HTTPS SSL

This project configures DNS subdomains and HTTPS on a load balancer. HAProxy terminates SSL using a Let's Encrypt certificate and redirects HTTP traffic to HTTPS.

## Environment

- Ubuntu 20.04 LTS
- HAProxy 2.0 (SSL termination needs 1.5 or higher)
- Nginx on `web-01` and `web-02`
- Certbot (Let's Encrypt)
- Domain: `joshuahotay.tech`

## Files

| File | Description |
|---|---|
| `0-world_wide_web` | Bash script that displays the record type and destination of the subdomains `www`, `lb-01`, `web-01` and `web-02`, or of one given subdomain |
| `1-haproxy_ssl_termination` | HAProxy configuration that accepts HTTPS traffic on port 443 and forwards it to the web servers |
| `2-redirect_http_to_https` | HAProxy configuration that also redirects HTTP traffic to HTTPS with a 301 |

## Usage

### 0-world_wide_web

```
./0-world_wide_web <domain> [subdomain]
```

All four subdomains:

```
$ ./0-world_wide_web joshuahotay.tech
The subdomain www is a A record and points to 13.40.24.204
The subdomain lb-01 is a A record and points to 13.40.24.204
The subdomain web-01 is a A record and points to 18.134.228.22
The subdomain web-02 is a A record and points to 18.170.29.61
```

One subdomain:

```
$ ./0-world_wide_web joshuahotay.tech web-02
The subdomain web-02 is a A record and points to 18.170.29.61
```

### HAProxy configuration files

Copy the file to `/etc/haproxy/haproxy.cfg` on `lb-01`, check it, and restart HAProxy:

```
sudo haproxy -c -f /etc/haproxy/haproxy.cfg
sudo service haproxy restart
```

Test:

```
curl -sI https://www.joshuahotay.tech
curl -sIL http://www.joshuahotay.tech
```

## Author

Joshua Stanley Hotay
