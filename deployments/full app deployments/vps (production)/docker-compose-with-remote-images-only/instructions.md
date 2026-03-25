# Fastapi, postgres and Nginx+React all in containers

Fastapi, postgres and Nginx+React all in containers, with Nginx forwarding backend requests to Uvicorn (for FastAPI) and
serving React's static files. Vite is no longer used for its dev server, only for building the bundle (dist folder).

---

## Setup overview:

### Entities:

- Containers:
    - Container for Postgres (official image)
    - Container for FastAPI running FastAPI's default dev server
    - Container for Nginx serving the static files of React
    - *Optional* - PgAdmin container (deployed on client machine)

### Communications:

- Browser ⇆ React:
    - Browser → Nginx (via localhost)
    - Nginx serves React's static files

- Browser ⇆ FastAPI:
    - Browser → Nginx in a container (via localhost)
    - Nginx in a container → Uvicorn in FastAPI container (via a docker network) → Uvicorn → FastAPI
    - FastAPI container → Postgres container (via Docker network)
    - *Optional* - PgAdmin container (accessed via browser) → Postgres container on vps (via port mapping)

---

## Steps

### 1. Create a new directory somewhere on your server and the place the provided .env files and docker-compose files inside.

    /some-directory    
        docker-compose.yml
        eap-backend
        eap-frontend


### 2. From inside the directory, run: ```docker compose up -d```


### 3. pgAdmin

#### 3.1 Run a pgAdmin container on your own computer (Regular pgAdmin can also be used, but it seems to be very slow)

```bash  
docker run --name eap-pgadmin-container \
  --network eap-docker-net \
  -p 5050:80 \
  -e PGADMIN_DEFAULT_EMAIL=admin@example.com \
  -e PGADMIN_DEFAULT_PASSWORD=admin \
  -d dpage/pgadmin4
```

---

#### 3.2 Go to localhost:5050 in the browser and log in with the email and password you ran the container with.

#### 3.3 Connect to the postgres container we ran (register server):

![img_1.png](img_1.png)

![img_2.png](img_2.png)

Go to the connection tab. Use the IP of the VPS as the host. For the port we'll use the VPS's port that we mapped to the
Postgres container's port. Also fill in the correct username and password, the ones you gave the postgres container.

![img_3.png](img_3.png)

---

### 4. Go to ```eap-it31.motoppdemo.nl``` and confirm the app is working.