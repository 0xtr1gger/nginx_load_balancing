# Nginx Load Balancing

This project demonstrates how to set up a load balancer using Nginx to distribute traffic between two static websites.

The project consists of:
- Two Flask applications running in Docker containers. The applications differ only in the `index.html` file content they serve to distinguish them easily. 
- An Nginx Docker container responsible for load-balancing.
- The Nginx configuration file, `nginx.conf`, mounted to the load-balancer container.
- A Docker Compose file that sets up the lab automatically.

To start the lab, run:

```bash
docker-compose up -d
```

Then navigate to `localhost:80`. Each time you update the page, you will see that Nginx redirects you to the alternate backend.

---

By default, Nginx is configured to use the Round Robin load-balancing algorithm:

```Python
# ...
http {
  upstream backend {
    # Round Robin
    server web_server1:5001;
    server web_server2:5002;
  }
# ...
```
To change the algorithm, substitute the comment line in the `upstream backend` block with the name of the algorithm you would like to set. 
Nginx supports the following load-balancing algorithms:

| Algorithm                                                                                    | Configuration                               | Description                                                                                                                                                                                                     |
| -------------------------------------------------------------------------------------------- | ------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Round Robin                                                                                  | Set by default, nothing should be specified | Requests are distributed evenly across the servers, with server weights considered (if configured).                                                                                                             |
| [Least Connections](https://nginx.org/en/docs/http/ngx_http_upstream_module.html#least_conn) | `least_conn;`                               | A request is routed to the server with the least number of currently active connections, with server weights considered (if configured).                                                                        |
| [IP Hash](https://nginx.org/en/docs/http/ngx_http_upstream_module.html#ip_hash)              | `ip_hash;`                                  | The method guarantees that requests from the same address get to the same server unless it is not available. Each request is directed to the server based on the hash of the source IP address of that request. |
| [Hash](https://nginx.org/en/docs/http/ngx_http_upstream_module.html#hash)                    | `hash $request_uri;`                        | Similar to IP Hash, but the hash value is calculated based on a user-defined key, such as a test string or variable; for example, a request URL.                                                                |
| [Random](https://nginx.org/en/docs/http/ngx_http_upstream_module.html#random)                | `random;`                                   | A request is routed to a randomly selected server.                                                                                                                                                              |

- [Nginx documentation](https://docs.nginx.com/nginx/admin-guide/load-balancer/http-load-balancer/)