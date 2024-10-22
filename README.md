<p align="center">
  <a href="http://nestjs.com/" target="blank"><img src="https://nestjs.com/img/logo-small.svg" width="120" alt="Nest Logo" /></a>
</p>

  <p align="center">A progressive <a href="http://nodejs.org" target="_blank">Node.js</a> framework for building efficient and scalable server-side applications.</p>
  
## Description

This is a NestJS application that provides an API for tracking cryptocurrency prices. The project utilizes Docker for containerization, Swagger for API documentation, and Moralis API for fetching cryptocurrency prices. It also includes an email notification system.

## Prerequisites

Ensure you have the following installed:
- Docker and Docker Compose
- Node.js and npm

## Setup

### 1. Clone the Repository

```bash
git clone git@github.com:your-username/repository.git
cd repository
```

### 2. Create the `.env` File

You need to create a `.env` file in the project root with the following content:

```bash
# Database Configuration
DB_HOST=db
DB_PORT=3306
DB_USERNAME=your_db_username
DB_PASSWORD=your_db_password
DB_DATABASE=your_db_name
PORT=3333

# Moralis API Key
MORALIS_API_KEY=your_moralis_api_key

# Email Configuration
EMAIL_HOST=smtp.your-email-host.com
EMAIL_PORT=587
USER_EMAIL=your-email@example.com
EMAIL_PASSWORD=your-email-password
```

Replace `your_db_username`, `your_db_password`, `your_db_name`, `your_moralis_api_key`, and email credentials with the correct values.

### 3. Create the Database

Before running the application, make sure you have created a new MySQL database that matches the credentials in your `.env` file.

### 4. Generate Moralis API Key

- Visit [Moralis](https://moralis.io/).
- Sign up and generate an API key.
- Add the generated API key to your `.env` file under `MORALIS_API_KEY`.

### 5. Build and Run with Docker

```bash
docker-compose up --build
```

This command will start the application and a MySQL database within Docker containers.

### 6. Access the Application

- API will be available at: `http://localhost:3333`
- API Documentation (Swagger) will be available at: `http://localhost:3333/api`

## API Documentation

The project uses Swagger for API documentation. After starting the application, you can access the documentation at:

```
http://localhost:3333/api
```

This provides an interactive interface to explore and test the available endpoints.

## Running Locally (Without Docker)

If you want to run the project locally without Docker:

### 1. Install Dependencies

```bash
npm install
```

### 2. Run the Application

```bash
# development
npm run start

# watch mode
npm run start:dev

# production mode
npm run start:prod
```

Make sure to have a MySQL database running and configured in the `.env` file before running the app locally.

## Testing

You can run the following commands to test the application:

```bash
# unit tests
npm run test

# e2e tests
npm run test:e2e

# test coverage
npm run test:cov
```

## Docker Configuration

This application is fully containerized using Docker. Below is the `docker-compose.yml` configuration:

```yaml
version: '3.8'

services:
  db:
    image: mysql:8.0
    ports:
      - '3306:3306'
    environment:
      MYSQL_ROOT_HOST: '%'
      MYSQL_DATABASE: ${DB_DATABASE}
      MYSQL_USER: ${DB_USERNAME}
      MYSQL_PASSWORD: ${DB_PASSWORD}
      MYSQL_ROOT_PASSWORD: ${DB_PASSWORD}
    volumes:
      - mysql_data:/var/lib/mysql
    restart: always
    healthcheck:
      test: ['CMD', 'mysqladmin', 'ping', '-h', '127.0.0.1']
      interval: 10s
      timeout: 5s
      retries: 5

  app:
    build:
      context: .
      dockerfile: Dockerfile
    ports:
      - '3333:3333'
    environment:
      MORALIS_API_KEY: ${MORALIS_API_KEY}
      USER_EMAIL: ${USER_EMAIL}
      EMAIL_PASSWORD: ${EMAIL_PASSWORD}
      EMAIL_PORT: ${EMAIL_PORT}
      EMAIL_HOST: ${EMAIL_HOST}
      DB_HOST: db
      DB_PORT: 3306
      DB_USERNAME: ${DB_USERNAME}
      DB_PASSWORD: ${DB_PASSWORD}
      DB_DATABASE: ${DB_DATABASE}
    depends_on:
      db:
        condition: service_healthy
    restart: always

volumes:
  mysql_data:
```

## Email Configuration

To enable email sending functionality, ensure the following environment variables are correctly configured in the `.env` file:

```bash
EMAIL_HOST=smtp.your-email-host.com
EMAIL_PORT=587
USER_EMAIL=your-email@example.com
EMAIL_PASSWORD=your-email-password
```

Make sure to use a valid SMTP server. You can use services like Gmail, SendGrid, or any other SMTP provider.