```
  ___ ___ ____   ____   ____  _____ ____ _______   __
 / _ \_ _|  _ \ / ___| |  _ \| ____/ ___|_   _\ \ / /
| | | | || | | | |     | |_) |  _| \___ \ | |  \ V / 
| |_| | || |_| | |___  |  _ <| |___ ___) || |   | |  
 \___/___|____/ \____| |_| \_\_____|____/ |_|   |_|  
                                                     
```
<hr>

## Introduction
There are many options for authenticating API calls, however, OAuth 2.0 authentication mechanism is the one being majorly used. Here, OAuth 2.0 access tokens are the authentication credentials passed from client to API server and typically carried as an HTTP header.

Keycloak supports OIDC (an extension to OAuth 2.0) and works as an IdP while authenticating a client.

The standard method for validating access tokens with an IdP is called token introspection. Nginx acts as an OAuth 2.0 Relying Party, sending access tokens to the IdP for validation i.e. token introspection, and only proxying requests that pass the validation process.

<img src="https://kevalnagda.github.io/assets/img/keycloak/1.png"/>

## Setup
### Step 1
- Add entry `127.0.0.1	localhost host.docker.internal` in  `/etc/hosts` file.

### Step 2
- `git clone https://github.com/rajat965ng/oidc.broker.git`
- `cd oidc.broker`
- `docker-compose up`

### Step 3
- Access the Keycloak admin portal at `http://localhost:3333`. After logging in, we create a new realm `myrealm` in order to add our clients.
  <img src="https://kevalnagda.github.io/assets/img/keycloak/3.png"/>
- Add new client `nginx`. Set `Valid Redirect URIs` to `http://localhost:3002/*`.
  <img src="https://kevalnagda.github.io/assets/img/keycloak/4.png"/>
- Need to add a secret key from the `Credentials` tab to the Nginx configuration file (.nginx/nginx.conf).
  <img src="https://kevalnagda.github.io/assets/img/keycloak/5.png"/>
- Add user details into `Users` required for authentication when a user tries to access any application.
  <img src="https://kevalnagda.github.io/assets/img/keycloak/6.png"/>
- Add user password
  <img src="https://kevalnagda.github.io/assets/img/keycloak/7.png"/>

### Step 4
- `docker-compose restart`
- Open Chrome browser incognito window
- Browse `app1` @ http://localhost:3002
- Browse `app2` @ http://localhost:4090