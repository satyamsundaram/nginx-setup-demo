### Goal
We setup Nginx server to:
- listen on ports 80 & 443, 
- redirect from http to https and 
- serve as a reverse proxy for multiple backends.

### Setup
- clone the repo and cd into it
- Find your public IP address by running `curl ifconfig.me` or `ifconfig` in the terminal and replace it in the `nginx.conf` file for the upstream apache backend servers.
- generate self-signed certificate and key files using the command below:
```bash
openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout key.pem -out cert.pem
# Enter the FQDN or IP address when asked for the Common Name (e.g. localhost, a.com, blog.a.com, etc.)
```

### Run
```bash
docker-compose up -d
docker-compose -f apache-docker-compose.yaml up -d
```

### Test
- Open your browser and visit `http://localhost` to see our custom page for the Ngix server and see it redirect to `https://localhost` automatically. If the browser prompts you about the certificate, accept it and proceed. 
- Visit `http://localhost:8081`, `http://localhost:8082` and `http://localhost:8083` to see the custom pages for the Apache backend servers.
- Visit `http://localhost:8080` to see the Nginx server reverse proxy the Apache backend servers. It does so using the Round Robin algorithm by default. You can change this by editing the `nginx.conf` file.