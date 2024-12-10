# BTP PostgreSQL Web Admin

## Introduction
If you use HANA on SAP BTP, you have HANA DB Explorer built-in. But what about PostgreSQL?

While locally running a DB explorer for BTP PostgreSQL requires SSH tunneling (which isn't very convenient), there's a better way. You can deploy pgAdmin4 Web directly to your BTP subaccount with just a few commands, allowing your whole team to access it conveniently.

## Prerequisites
- CF CLI installed and configured
- Developer role in your BTP space
- Access to your BTP PostgreSQL instance

## Deployment Steps

### 1. Verify Docker Support
Check if Docker support is enabled in your environment:
```bash
cf feature-flags | grep diego_docker
```

If not enabled, and you have admin privileges:
```bash
cf enable-feature-flag diego_docker
```

### 2. Deploy pgAdmin
Deploy the pgAdmin web interface:
```bash
cf push pgadmin-web --docker-image dpage/pgadmin4:latest -m 1G -k 1G --health-check-type process --no-start
```

### 3. Configure Environment
Set the required environment variables:
```bash
cf set-env pgadmin-web PGADMIN_DEFAULT_EMAIL <your-admin-email>
cf set-env pgadmin-web PGADMIN_DEFAULT_PASSWORD <your-secure-password>
cf set-env pgadmin-web PGADMIN_LISTEN_ADDRESS 0.0.0.0
```

### 4. Start the Application
```bash
cf start pgadmin-web
```

## Access
After deployment, you can access pgAdmin through the route displayed in your CF console, typically:
`https://pgadmin-web.<your-cf-domain>`

Log in using the email and password you set in the environment variables.

## Notes
- For production environments, consider using a specific version tag instead of `latest`
- Ensure you use a strong password for the admin account
- The application uses 1GB of memory by default

## Contributing
Feel free to contribute to this project by submitting issues or pull requests.

## License
MIT License 
