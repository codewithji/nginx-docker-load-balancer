# NGINX Docker Load Balancer

This project demonstrates a simple round-robin load balancing setup using NGINX, Docker, and a basic Express.js app.

The setup consists of an NGINX server acting as a load balancer that distributes incoming HTTP requests to a pool of Express.js servers in a round-robin manner. Each Express.js server instance runs in a separate Docker container.

## Steps to run locally

### 1. Build the Express.js app image

```bash
docker build . -t nodeapp
```

### 2. Run Express.js app containers

```bash
docker run --hostname nodeapp1 --name nodeapp1 -d nodeapp
docker run --hostname nodeapp2 --name nodeapp2 -d nodeapp
docker run --hostname nodeapp3 --name nodeapp3 -d nodeapp
```

### 3. Create a Docker network

```bash
docker network create backendnet
```

### 4. Connect containers to the network

```bash
docker network connect backendnet nodeapp1
docker network connect backendnet nodeapp2
docker network connect backendnet nodeapp3
```

### 5. Run NGINX container

```bash
docker run --name nginx --hostname ng1 -p 80:8080 -v /PATH/TO/THIS/REPO/nginx.conf:/etc/nginx/nginx.conf -d nginx
```

### 6. Connect NGINX container to the network

```bash
docker network connect backendnet nginx
```

### 7. Test on browser

Open a web browser and navigate to `http://localhost:80`. Refresh the page multiple times to see the load balancer in action, distributing requests to different Express.js server instances.
