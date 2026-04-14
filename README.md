# Cloudboosta WordPress Deployment (Docker Compose)

## Project Overview
This project demonstrates how Cloudboosta standardizes WordPress deployments using Docker Compose.
The goal is to ensure consistent application behavior across different operating systems by
containerizing the WordPress application and its dependencies.

The setup includes:
- WordPress application
- MySQL database
- phpMyAdmin (for database management during development)

---

## Architecture
- **WordPress** runs in a Docker container and serves the website.
- **MySQL** runs in a separate container and stores WordPress data.
- **phpMyAdmin** provides a web-based interface to manage the MySQL database.
- All services communicate through Docker’s internal network.
- Persistent volumes are used to prevent data loss.

---

## Technologies Used
- Docker
- Docker Compose
- WordPress
- MySQL
- phpMyAdmin

---

## Project Structure
```text
.
├── cloudboosta_wordpress/
│   └── README.md
├── data/
│   ├── db/
│   └── wp-content/
├── docker-compose.yml
└── wordpress/


Prerequisites:
Docker installed
Docker Compose installed


How to Run:
Clone the repository
Create a .env file
Run:  "docker-compose up -d"