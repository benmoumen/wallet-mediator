# Aries Cloud Agent Mediator

This project enables the deployment of an Aries Cloud Agent that serves as a Mediator, facilitating the delivery of DIDComm messages to Mobile Digital Wallets.

## Project Structure

- `.env`: Environment variables file.
- `.env.sample`: Sample environment variables file.
- `.gitattributes`: Git attributes configuration.
- `.gitignore`: Git ignore configuration.
- `docker-compose.yml`: Docker Compose configuration file.

## Getting Started

### Prerequisites

- Docker
- Docker Compose

### Environment Variables

Copy the `.env.sample` file to `.env` and fill in the required values.

### Docker Compose

To start the services, run:

`docker-compose up`

### Services

- **mediator**: The Aries Cloud Agent Mediator service.
- **mediator-db**: PostgreSQL database for the mediator.

### Configuration

The `docker-compose.yml` file includes the following configurations:

- **Mediator Service**:

  - Image: `ghcr.io/openwallet-foundation/acapy-agent:py3.12-1.1.0`
  - Depends on: `mediator-db`
  - Entrypoint: `/bin/bash`
  - Command: Starts the ACA-Py with various configurations.

- **Mediator Database**:
  - Image: `postgres:15`
  - Environment variables:
    - `POSTGRES_USER`
    - `POSTGRES_PASSWORD`
    - `POSTGRES_ADMIN_PASSWORD`
  - Volumes: `mediator-wallet:/var/lib/pgsql/data:z`
  - Healthcheck: `pg_isready -U postgres`

### Networks and Volumes

- **Networks**:

  - `mediator-network`

- **Volumes**:
  - `mediator-wallet`

### Healthchecks

- **Mediator Service**:

  - Test: `/bin/bash -c "</dev/tcp/mediator/3000"`
  - Interval: 5s
  - Timeout: 5s
  - Retries: 5

- **Mediator Database**:
  - Test: `pg_isready -U postgres`
  - Interval: 3s
  - Timeout: 3s
  - Retries: 5

## Environment Variables

The following environment variables are used in the project:

- `LOG_LEVEL`: Log level (default: `INFO`)
- `POSTGRESQL_USER`: PostgreSQL user (default: `postgres`)
- `POSTGRESQL_PASSWORD`: PostgreSQL password
- `POSTGRESQL_ADMIN_USER`: PostgreSQL admin user (default: `postgres`)
- `POSTGRESQL_ADMIN_PASSWORD`: PostgreSQL admin password
- `MEDIATOR_WALLET_NAME`: Mediator wallet name (default: `mediator`)
- `MEDIATOR_WALLET_KEY`: Mediator wallet key
- `MEDIATOR_AGENT_LABEL`: Mediator agent label (default: `Mediator`)
- `MEDIATOR_SERVER_NAME`: Mediator server name
- `MEDIATOR_AGENT_HTTP_ADMIN_PORT`: Mediator agent HTTP admin port (default: `5680`)
- `MEDIATOR_AGENT_HTTP_IN_PORT`: Mediator agent HTTP inbound port (default: `5681`)
- `MEDIATOR_AGENT_WS_IN_PORT`: Mediator agent WebSocket inbound port (default: `5682`)

## License

This project is licensed under the MIT License.
