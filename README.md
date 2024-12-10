# Introduction
If you use HANA on BTP, you have HANA DB Explorer. 
What if you use PostgreSQL?

Locally running a DB explorer for a BTP-PostgreSQL requires you to SSH tunnel, which is not super convenient.

You can deploy a PHPMyAdmin-Web to your BTP subaccount with just a couple of commands and then add your BTP PostgreSQL DB to it. That way, your whole team can access it conveniently.

# CLI commands
Rum below commands sequentially. You need to have a Developer role in the space. 

## Check if Docker support is enabled
cf feature-flags | grep diego_docker

## If not enabled, enable it (requires admin privileges)
cf enable-feature-flag diego_docker

## Deploy pgAdmin. 
cf push pgadmin-web --docker-image dpage/pgadmin4:latest -m 1G -k 1G --health-check-type process --no-start

## Set environment variables
cf set-env pgadmin-web PGADMIN_DEFAULT_EMAIL <Provide a user ID that you will use to connect to PHPMyAdmin>
cf set-env pgadmin-web PGADMIN_DEFAULT_PASSWORD <Provide a password>
cf set-env pgadmin-web PGADMIN_LISTEN_ADDRESS 0.0.0.0

# Start the application
cf start pgadmin-web
