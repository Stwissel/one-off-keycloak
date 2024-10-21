# Keycloak Initialization Script

The repository contains a [Bruno HTTP Client](https://usebruno.com) collection and environment to initialize a [KeyCloak](https://www.keycloak.org/) container. It can be run manually or in a CI/CD environment.

## Keycloak setup

```shell
docker run --rm -p 8080:8080 \
       --name testcloak \
       -e KEYCLOAK_ADMIN=admin \
       -e KEYCLOAK_ADMIN_PASSWORD=password \
       quay.io/keycloak/keycloak:latest start-dev
```

This creates a KeyCloak instance without persistence.

## Use the Bruno GUI

make sure [you know the Bruno HTTP Client](https://docs.usebruno.com/).

- Open the environment `KeycloakEnv`, check the values and provide values for `ADMIN_PASSWORD` and `USER_PASSWORD`
- Run the entries in `AutomationScript` in sequence
- When you wait for a short while between calls, the access token will be expired and you need to run `AdministratorRefresh` (Could be automated, but where's the fun in that?)
- The user directory contains a call that could be used with a CSV file to create more users

## CI/CD or command line use

You need to have the [Bruno CLI](https://docs.usebruno.com/bru-cli/overview) installed. Then run (unse CI/CD env variables for the passwords):

```shell
bru run AutomationScript --env KeycloakEnv \
    --env-var ADMIN_PASSWORD=password \
    --env-var USER_PASSWORD=password \
    --reporter-html results.html \
    --reporter-junit results.xml
```

This will create the realm, the client and one user. To create additional users run:

```shell
bru run User --env KeycloakEnv \
    --env-var ADMIN_PASSWORD=password \
    ---csv-file-path users.csv \
    --reporter-html results.html
```

### CSV format

```csv
NAME,FIRSTNAME,LASTNAME,EMAIL,PASSWORD,CN
peterpan,Peter,Pan,peter@neverland.email,tinkerbell69,CN=Peter Pan/O=LostBoys
```

## Blog article

The steps are [described here](https://wissel.net/blog/2024/10/onetime-idp-with-keycloak.html)

Good luck!
