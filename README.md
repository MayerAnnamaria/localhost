
# Sage 10 WordPress Development Environment on macOS

Local development setup for working with a **WordPress project using the Sage 10 starter theme** on an **Apple Silicon (M1) Mac running macOS Big Sur**.

This repository documents the local web-server setup, including **Homebrew, Apache, PHP, virtual hosts, local SSL certificates, MySQL, and phpMyAdmin**. It also serves as a place to record the WordPress and Sage 10 installation workflow.

> **Documentation status:** The Apache and virtual-host configuration notes are available. The full WordPress and Sage 10 installation procedure has not yet been documented here.

## Table of Contents

1. [Development Environment](#development-environment)
2. [Setup Documentation](#setup-documentation)
3. [Local Development Workflow](#local-development-workflow)
4. [Sage 10 Installation](#sage-10-installation)
5. [References](#references)

---

## 1. Development Environment

| Component | Purpose |
|-----------|---------|
| macOS Big Sur (Apple M1) | Local development operating system |
| Homebrew | Package management |
| Apache HTTP Server | Local web server |
| PHP | WordPress runtime |
| Virtual Hosts | Local `.test` domains for separate projects |
| Local SSL Certificates | HTTPS for local development |
| MySQL | WordPress database |
| phpMyAdmin | Database administration |
| WordPress | Content management system |
| Sage 10 | WordPress starter theme |

---

## 2. Setup Documentation

For the original installation commands and configuration examples, see:

**[macOS Local Web Server Setup](macos_local_web_server_setup.md)**

The guide covers:

- Checking Homebrew and installing Apache.
- Configuring Apache ports, document root, and URL rewriting.
- Starting, stopping, and troubleshooting Apache.
- Setting up local virtual hosts and `.test` domains.
- Configuring local HTTPS certificates.
- References for PHP, MySQL, and phpMyAdmin installation.

---

## 3. Local Development Workflow

The documented environment is intended to support multiple local WordPress sites with their own project directories and development domains.

Each local project can use:

- A dedicated project directory.
- A custom `.test` development domain.
- An Apache virtual host configuration.
- A locally generated SSL certificate.
- A MySQL database for WordPress.

Refer to the [setup documentation](macos_local_web_server_setup.md) for the exact commands, configuration paths, and examples.

> **Compatibility Note:** These notes describe a specific local development environment. Paths, dependencies, and package behavior may differ across macOS versions and Homebrew installations. The commands have not been independently tested as part of this README.

---

## 4. Sage 10 Installation

**Status: To be documented**

The current notes do not yet contain a complete, verified installation procedure for WordPress and the Sage 10 starter theme.

Future documentation can cover:

1. Installing WordPress locally.
2. Preparing the Sage 10 development environment.
3. Installing theme dependencies.
4. Configuring the local WordPress project.
5. Compiling frontend assets.
6. Running the local theme development workflow.

---

## 5. References

- [Homebrew](https://brew.sh/)
- [WordPress](https://wordpress.org/)
- [Roots Sage](https://roots.io/sage/)
- [Apache and Multiple PHP Versions on macOS](https://getgrav.org/blog/macos-ventura-apache-multiple-php-versions)
- [Local SSL with Apache on macOS](https://getgrav.org/blog/macos-ventura-apache-ssl)

---

**Project:** WordPress Sage 10 Local Development Environment  
**Platform:** macOS Big Sur — Apple Silicon (M1)  
**Documentation:** Local web server configuration and development setup
