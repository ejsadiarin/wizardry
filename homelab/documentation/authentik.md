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

## Forward Auth Authentik Proxy for Web Apps/Services

*refer to official documentation for the Traefik config*

1. edit main `traefik.yml` file to add file
```yaml
...
providers:
  docker:
    endpoint: "unix:///var/run/docker.sock"
    exposedByDefault: false
  file:
    filename: /config.yml
...
```


