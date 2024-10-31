# Authentik Documentation
- ref: [https://youtu.be/N5unsATNpJk?si=7xCBs01pyuwULeoM](https://youtu.be/N5unsATNpJk?si=7xCBs01pyuwULeoM)

##  Post-Installation

1. Create a new custom user as new admin
- admin interface -> directory -> users -> create user
- in the same users panel -> "username" -> update password

2. Deactivate akadmin (default admin)
- admin interface -> directory -> users -> deactivate

3. enable MFA (TOTP or any) 
- user interface -> settings -> MFA Devices -> Enroll

## Connect OAuth Services/OAuth Login for Services (ex. Portainer, Proxmox, etc.)

*refer to official documentation for ALL available services and how to integrate them*

- This will automatically create a user for the services
- NOTE: all apps need a Provider and Application

- Always need to configure permissions/admin access separately on the Web Interface of the Services
    - for the OAuth Authentik-created user

7. Configure admin permissions on the service (optional)

## Forward Auth Authentik Proxy for Web Apps/Services (as Traefik Middleware for Auth)

- we use Traefik as reverse proxy (the best!)
- we will use authentik as a middleware

*refer to official documentation for the Traefik config*

1. edit main `traefik.yml` file to add `config.yml` file

```yaml
# traefik/data/traefik.yml
...
providers:
  docker:
    endpoint: "unix:///var/run/docker.sock"
    exposedByDefault: false
  file:
    filename: /config.yml
...
```
or do something like:
```yaml
  file:
    directory: /etc/traefik/conf/
    watch: true
```

2. mount the `config.yml` on traefik container
```yaml

# traefik/docker-compose.yml
traefik:
    ...
    volumes:
      - /etc/localtime:/etc/localtime:ro
      - /var/run/docker.sock:/var/run/docker.sock:ro
      - ./data/traefik.yml:/traefik.yml:ro
      - ./data/acme.json:/acme.json
      - ./data/config.yml:/config.yml:ro # add this
    ...
```

3. configure the authentik middleware in the `config.yml` 

- this will be added to the traefik dynamic config
- *NOTE: authentik-server is the container_name in the forwardAuth.address*
    - go to your authentik's `docker-compose.yml` and find the `container_name` of the server

```yaml
# traefik/data/config.yml
http:
  middlewares:
    authentik:
      forwardAuth:
        address: http://authentik-server:9000/outpost.goauthentik.io/auth/traefik
        trustForwardHeader: true
        authResponseHeaders:
          - X-authentik-username
          - X-authentik-groups
          - X-authentik-email
          - X-authentik-name
          - X-authentik-uid
          - X-authentik-jwt
          - X-authentik-meta-jwks
          - X-authentik-meta-outpost
          - X-authentik-meta-provider
          - X-authentik-meta-app
          - X-authentik-meta-version
```

4. add docker labels to the service you want to have the authentik middleware proxy
- in this example, we will use nginx
- add `traefik.http.routers.<service>.middlewares=authentik@file`
    - `authentik` is the middleware name in the `traefik/data/config.yml`
    - `@file` is the provider (see ``)
```yaml
# nginx/docker-compose.yml
services:
  nginx:
    image: nginxdemos/nginx-hello
    labels:
      - "traefik.enable=true"
      - "traefik.http.routers.nginx.rule=Host(`nginx.local.example.com`)"
      - "traefik.http.routers.nginx.entrypoints=https"
      - "traefik.http.routers.nginx.tls=true"
      - "traefik.http.routers.nginx.middlewares=authentik@file" # add this
      - "traefik.http.services.nginx.loadbalancer.server.port=8080"
    networks:
      - proxy

networks:
  proxy:
    external: true
```

5. go to the Authentik Admin Interface and register the Provider & Application
- example:
    auth.example.com -> Admin Interface (Dashboard) -> Applications -> Providers
- In Providers:
    - Create -> select Proxy Provider
    - Name: <any-name> (ex. nginx)
    - Authorization flow: ...explicit-consent (so there is a "I agree page")
    - select Forward Auth (single application)
    - External host: <domain-of-the-service> (ex. https://nginx.local.example.com)
    - Then finally click create.
- In Applications:
    - Create
    - Name: <any-name> (ex. nginx)
    - Slug: <any-name-or-same-as-name> (ex. nginx)
    - Provider: select the one you created (ex. nginx)
    - Then finally click create.
- In Outposts:
    - Edit the `authentik Embedded Outpost`
    - click on the Application (ex. nginx) -> select it by pressing the *double right arrow button*
    - then update.
    - NOTE: this is needed so that the application/service will be picked up by the outpost

6. Test the service by going to the domain (ex. nginx.local.example.com)
- this should automatically try to authenticate using Authentik
