# Ansible role: nginx reverse proxy

Simple Ansible role that configures a reverse proxy using the nginx web server.

Author: Colin Murphy, 2024

## Role variables

Role variables:

* `nginx_proxy_port`: HTTP port, defaults to 80
* `nginx_proxy_port_ssl`: HTTPS port, defaults to 443
* `nginx_proxy_redirect_ssl`: Redirects all http virtual hosts to https
* `nginx_proxy_default_cert`: Path to default certificate
* `nginx_proxy_default_cert_key`: Path to default certificate key
* `nginx_proxy_vhosts`: List of proxy virtual hosts
* `nginx_proxy_ssl_config`: SSL configuration from the [Mozilla SSL Configuration Generator](https://ssl-config.mozilla.org/). Defaults to `modern`, with `intermediate` available as an option.
* `nginx_proxy_enable_hsts`: Enable HTTP Strict Transport Security (HSTS). Defaults to `true`.
* `nginx_proxy_dhparam_path`: Path to dhparams. **dhparams are not used with the `modern` SSL configuration**
* `nginx_proxy_dhparam_size`: dhparam size, defaults to 2048.

Proxy vhost variables:

* `servername`: hostname of the reverse proxy
* `https`: If set to true, HTTPS is enabled for this virtual host
* `proxy_host`: Address of the service to proxy to, example http://127.0.0.1:8080
* `allowed_ips`: List of IP addresses, including IP ranges in CIDR notation, to allow to the proxied service
* `cert_path`: Path to certificate
* `cert_key_path`: Path to certificate key
* `extra_params`: Extra configuration parameters
* `proxy_params`: Extra configuration parameters placed in the `location` directive of the virtual host
* `ssl_verify`: Verify SSL certificate of proxied service. Defaults to true

## Example Playbook

```yaml
- hosts: proxy
  tasks:
    - name: Nginx proxy server
      include_role: name=nginx-proxy
      vars:
        nginx_proxy_default_cert: /etc/ssl/certs/ssl-cert-snakeoil.pem
        nginx_proxy_default_cert_key: /etc/ssl/private/ssl-cert-snakeoil.key
        nginx_proxy_vhosts:
          - servername: proxmox.example.org
            https: true
            proxy_host: http://192.168.88.10:8006
            allowed_ips:
              - "192.168.88.0/24"
            ssl_verify: false
```

## Dependencies

* [nginxinc.nginx](https://galaxy.ansible.com/nginxinc/nginx)
