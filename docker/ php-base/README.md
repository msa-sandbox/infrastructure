# PHP Base Image

This directory contains the base Docker image used by multiple MSA services (e.g. `auth`, `crm`).

The image provides:

* PHP 8.2 FPM runtime
* Common extensions: `pdo_mysql`, `zip`, `pcntl`, `redis`, `rdkafka`
* Composer (copied from the official `composer:2` image)
* Non-root `appuser` (UID 1000) for secure container runtime

---

## Build

### Local build

Run the following command from this directory:

```bash
  docker build -t alslob/msa-sandbox:php-base-8.2 .
```

This creates a local image tagged `alslob/msa-sandbox:php-base-8.2`.

---

## Push to Docker Hub

After building, log in to Docker Hub and push the image:

```bash
    docker login
    
    docker push alslob/msa-sandbox:php-base-8.2
```

The image will appear under your Docker Hub repository:
[https://hub.docker.com/repository/docker/alslob/msa-sandbox](https://hub.docker.com/repository/docker/alslob/msa-sandbox)

---

## Usage in other services

In service Dockerfiles (e.g. `auth`, `crm`):

```dockerfile
FROM alslob/msa-sandbox:php-base-8.2 AS base
WORKDIR /var/www/app
COPY . .
RUN composer install --no-dev --optimize-autoloader
CMD ["php-fpm"]
```

This setup allows:

* Shared PHP runtime across multiple services
* Smaller rebuilds (only source layers change)
* Consistent environment across the stack
