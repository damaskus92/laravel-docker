# Laravel 11 with Docker

This guide is a Laravel 11 project setup using Docker that includes a `docker-compose.yml` configuration and `Dockerfile` to simplify development.

## Prerequisites

Ensure the following tools are installed on your system:

- [Docker](https://www.docker.com/)
- [Docker Compose](https://docs.docker.com/compose/)
- [Git](https://git-scm.com/)

## Clone the Repository

First, clone the repository containing the Laravel project:

```bash
git clone git@github.com:damaskus92/laravel-docker.git

cd laravel-docker
```

## Environment Configuration

1. Create a `.env` file

    Laravel requires a `.env` file file for configuration. Copy the example `.env` file:

    ```bash
    cp .env.example .env
    ```

2. Update Environment Variables.

    Update the database configuration in the `.env` file to match the Docker MySQL service:

    ```bash
    DB_CONNECTION=mysql
    DB_HOST=db
    DB_PORT=3306
    DB_DATABASE=laravel
    DB_USERNAME=root
    DB_PASSWORD=password
    ```

## Build and Start Containers

Use Docker Compose to build and start the containers:

```bash
docker-compose up --build -d
```

This command will:

- Build the Docker image for the `app` service using the `Dockerfile`.
- Start containers for `app`, `webserver`, `db`, and `phpmyadmin`.

## Access Services

- **Laravel Application**: [http://localhost:8000](http://localhost:8000)
- **phpMyAdmin**: [http://localhost:8001](http://localhost:8001)

## Install Laravel Dependencies

Once the containers are running, execute the following command inside the `app` container to install dependencies:

```bash
docker exec -it laravel-app composer install
```

## Generate Application Key

Generate the application key required for Laravel:

```bash
docker exec -it laravel-app php artisan key:generate
```

## Run Migrations

Run the database migrations to set up your database schema:

```bash
docker exec -it laravel-app php artisan migrate
```

## Using Artisan Commands

You can run Laravel Artisan commands inside the `app` container. For example:

```bash
docker exec -it laravel-app php artisan <command>
```

## Stop and Remove Containers

To stop all running containers, use:

```bash
docker-compose down
```

## Rebuild and Restart Containers

If you make changes to the `Dockerfile` or `docker-compose.yml`, rebuild the containers:

```bash
docker-compose up --build -d
```

## Troubleshooting

1. Permissions Issues: Ensure the `storage` and `bootstrap/cache` directories are writable:

    ```bash
    docker exec -it laravel-app chmod -R 775 storage bootstrap/cache

    ```

2. Container Logs: Check logs for any errors:

    ```bash
    docker logs <container_name>
    ```

    > Replace `<container_name>` with the name of the container, e.g., `laravel-app`.

## File Structure Recap

The repository structure is as follows:

```bash
laravel-docker/
├── docker/
│   ├── mysql/
│   │   └── my.cnf
│   ├──nginx/
│   │   └── default.conf
│   └── php/
│       └── local.ini
├── .env
├── docker-compose.yml
└── Dockerfile
```

---

### Happy Coding! 🚀
