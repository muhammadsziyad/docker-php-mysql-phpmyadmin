# Project: PHP, MySQL, and phpMyAdmin with Docker

## Overview
This project sets up a Docker environment with PHP, MySQL, and phpMyAdmin using Docker Compose. Docker Compose allows you to define and run multi-container Docker applications with ease. We'll also publish the Docker image to Docker Hub.

## Prerequisites
1. Docker: Ensure Docker is installed on your machine. Follow the Docker Installation Guide.
2. Docker Compose: Docker Compose should be installed. Follow the Docker Compose Installation Guide.
3. Docker Hub Account: Create a Docker Hub account at Docker Hub.

## Project Structure

```bash
my-php-mysql-app/
│
├── docker-compose.yml
├── Dockerfile
├── src/
│   └── index.php
└── .env
```

# Step-by-Step Instructions
1. Create the Project Directory. Create a new directory for the project:

```bash
mkdir my-php-mysql-app
cd my-php-mysql-app
```

2. Create the docker-compose.yml File
Create a file named docker-compose.yml in the project directory with the following content:

```yml
version: '3.8'

services:
  db:
    image: mysql:5.7
    container_name: mysql-db
    environment:
      MYSQL_ROOT_PASSWORD: rootpassword
      MYSQL_DATABASE: mydatabase
      MYSQL_USER: user
      MYSQL_PASSWORD: userpassword
    ports:
      - "3306:3306"
    volumes:
      - db-data:/var/lib/mysql

  php:
    build: .
    container_name: php-app
    ports:
      - "80:80"
    depends_on:
      - db
    volumes:
      - ./src:/var/www/html

  phpmyadmin:
    image: phpmyadmin/phpmyadmin
    container_name: phpmyadmin
    ports:
      - "8080:80"
    environment:
      PMA_HOST: db
      PMA_PORT: 3306
    depends_on:
      - db

volumes:
  db-data:
```

3. Create the Dockerfile
Create a file named Dockerfile in the project directory with the following content:

```Dockerfile
# Use the official PHP image
FROM php:7.4-apache

# Install necessary PHP extensions
RUN docker-php-ext-install mysqli pdo pdo_mysql

# Set the working directory
WORKDIR /var/www/html
```

4. Create the PHP Source Directory
Create a directory named src in the project directory. Inside src, create a file named index.php with the following content:

```php
<?php
phpinfo();
?>
```

6. Build and Run the Docker Containers
In the project directory, run the following command to build and start the Docker containers:

```bash
docker-compose up --build
```

This command builds the Docker image for PHP and starts all containers (MySQL, PHP, and phpMyAdmin).

7. Test Your Setup
PHP Application: Open your browser and navigate to http://localhost to see the PHP info page. phpMyAdmin: Open your browser and navigate to http://localhost:8080 to access phpMyAdmin. Use the following credentials:

Server: db
Username: user
Password: userpassword

8. Publish to Docker Hub
To publish your Docker image to Docker Hub, follow these steps:

    1. Log In to Docker Hub:
    ```bash
    docker login
    ```

    2. Tag the Docker Image:
    Assuming you are in the project directory, tag your Docker image with your Docker Hub username and repository name:

    ```bash
    docker tag my-php-mysql-app_php-app yourusername/my-php-mysql-app:latest
    ```
    Replace yourusername with your Docker Hub username.

    3. Push the Docker Image:
    ```bash
    docker push yourusername/my-php-mysql-app:latest
    ```

# Conclusion
You have successfully created a Docker project with PHP, MySQL, and phpMyAdmin, and published the Docker image to Docker Hub. This setup demonstrates a typical stack for web development and showcases your ability to manage multi-container Docker applications.

# Additional Resources
Docker Documentation
Docker Compose Documentation
Docker Hub