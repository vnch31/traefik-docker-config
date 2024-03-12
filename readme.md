# Traefik configuration

> This is a simple example of a Traefik configuration file. It enables the web UI, sets up a basic redirect from HTTP to HTTPS, and configures a certificate resolver for Let's Encrypt.

## Instruction

### Traefik

1. Create a new `.env` file by copying the `.env.example` file and fill with the correct information.
2. Create a new docker network called `traefik_network` by running `docker network create traefik_network`.h
3. Run `docker compose up -d` to start the Traefik container.
4. You can now access the Traefik web UI at `https://traefik.<your-domain>`.

> To stop Traefik you can simply run `docker compose down`.

### Add new service

To add a new service to Traefik, you need to have an image running in a container.
Then you can create a new `docker-compose-<service-name>.yml` file in the `services` directory and add the following labels to the service:

```yaml
labels:
  - "traefik.enable=true"
  - "traefik.http.routers.<service-name>.rule=Host(`<service-domain>`)"
  - "traefik.http.routers.<service-name>.entrypoints=websecure"
  - "traefik.http.routers.<service-name>.tls.certresolver=letsencryptresolver"
  - "traefik.http.routers.<service-name>.tls=true"
```

Now you can run `docker compose -f docker-compose-<service-name>.yml up -d` to start the service and it will be automatically added to Traefik.
