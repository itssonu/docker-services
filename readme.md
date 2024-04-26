# Docker Compose Setup for PostgreSQL and pgAdmin

This Docker Compose configuration sets up a PostgreSQL database server and pgAdmin web interface for managing the database. It uses Docker containers to run both services.

## Services

### PostgreSQL Service

- **Image**: postgres:16
- **Volumes**: Mounts a local directory `./postgresql` to the container's data directory `/var/lib/postgresql/data` to persist database data.
- **Ports**: Maps container port 5432 to host port 5432 to allow connections to the PostgreSQL database.
- **Environment Variables**:
  - `POSTGRES_DB`: Sets the name of the initial database to be created (`initialDB` in this case).
  - `POSTGRES_USER`: Sets the username for accessing the database (`postgres` in this case).
  - `POSTGRES_PASSWORD`: Sets the password for the specified user (`root` in this case).

### pgAdmin Service

- **Image**: dpage/pgadmin4
- **Volumes**: Mounts a local directory `./pgadmin` to the container's data directory `/var/lib/pgadmin` to persist pgAdmin configuration and data.
- **Ports**: Maps container port 80 to host port 5050 to access the pgAdmin web interface.
- **Environment Variables**:
  - `PGADMIN_DEFAULT_EMAIL`: Sets the default email for the pgAdmin login (`pgadmin@sonu.info` in this case).
  - `PGADMIN_DEFAULT_PASSWORD`: Sets the default password for the pgAdmin login (`pgadmin` in this case).

## Usage

1. Make sure you have Docker and Docker Compose installed on your system.
2. Clone this repository to your local machine.
3. Navigate to the directory containing the `docker-compose.yml` file.
4. Run the following command to start the services:
`docker-compose up -d`
5. Access pgAdmin by opening a web browser and navigating to `http://localhost:5050`.
- Use the email `pgadmin@sonu.info` and password `pgadmin` to log in.
6. Configure a new server connection in pgAdmin to connect to the PostgreSQL database server:
- Host: `postgres` (the name of the PostgreSQL service defined in the `docker-compose.yml` file)
- Username: `postgres` (or the username specified in the `POSTGRES_USER` environment variable)
- Password: `root` (or the password specified in the `POSTGRES_PASSWORD` environment variable)
- Database: `initialDB` (or the name specified in the `POSTGRES_DB` environment variable)
- Save the connection and connect to the server.

## Notes

- The services are configured to restart automatically (`restart: always`), ensuring they start up again if the Docker daemon restarts.
- Both services are connected to the same network (`pg_network`), allowing them to communicate with each other.
- Ensure that the specified ports (5432 and 5050) are not already in use on your host machine before starting the services.
- For production use, consider securing your PostgreSQL database and pgAdmin interface with appropriate security measures such as strong passwords, network restrictions, and SSL/TLS encryption.

