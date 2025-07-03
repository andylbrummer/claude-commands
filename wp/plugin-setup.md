# WordPress Plugin Development Container Setup Guide

This guide provides detailed instructions for setting up a containerized WordPress development environment optimized for plugin development using Docker or Podman.

## Table of Contents
1. [Prerequisites](#prerequisites)
2. [Project Structure](#project-structure)
3. [Complete Template Files](#complete-template-files)
4. [Container Configuration](#container-configuration)
5. [User Permissions and File Ownership](#user-permissions-and-file-ownership)
6. [Volume Mapping Strategy](#volume-mapping-strategy)
7. [Step-by-Step Setup](#step-by-step-setup)
8. [Development Workflow](#development-workflow)
9. [Troubleshooting](#troubleshooting)
10. [Security Considerations](#security-considerations)

## Important Note for Podman Users
This setup includes specific configurations for Podman compatibility:
- SELinux labels (`:Z` suffix) on all volumes
- Security options to disable SELinux labels for containers
- User namespace handling for rootless containers
- Compatible with both Docker and Podman through automatic detection
- **Collision Prevention**: Unique container names and volumes using random suffixes
- **Random Port Assignment**: Automatic generation of non-conflicting ports
- **Non-privileged ports**: Apache configured to use port 8080 instead of 80
- **Environment Variable Limitations**: Podman doesn't expand environment variables in compose files like Docker does

### Critical Podman Differences
⚠️ **IMPORTANT**: Podman handles environment variables differently than Docker:
- Podman doesn't automatically expand `${VARIABLE:-default}` syntax in compose files
- Environment variables must be exported in your shell or set in .env file
- Some Makefile commands may require manual environment variable substitution

## Prerequisites

- Docker or Podman installed
- Docker Compose (for Docker) or podman-compose (for Podman)
- GNU Make
- Node.js and npm (for plugin build tools)
- Basic understanding of WordPress development

## Project Structure

```
project-root/
├── .env                    # Environment variables (create from .env.example)
├── .env.example            # Example environment configuration
├── .gitignore              # Git ignore file
├── docker-compose.yml      # Container orchestration
├── Dockerfile              # Custom WordPress image
├── Makefile                # Development commands
├── wp.sh                   # WP-CLI passthrough script
├── scripts/                # Initialization scripts
│   ├── init-wordpress.sh   # WordPress setup
│   ├── wp-cron.sh          # Cron configuration
│   ├── fix-permissions.sh  # Permission fixes for Podman
│   ├── create-pages.sh     # Optional: Create default pages
│   └── enable-svg.sh       # Optional: Enable SVG uploads
├── config/                 # Configuration files
│   ├── php.ini             # Custom PHP settings
│   └── uploads.ini         # Upload limits
├── tests/                  # Test files
│   ├── e2e/                # Playwright E2E tests
│   │   └── example.spec.js # Example test
│   └── phpunit/            # PHPUnit tests
│       └── bootstrap.php   # PHPUnit bootstrap
└── your-plugin/            # Plugin development directory
    ├── build/              # Compiled assets
    ├── src/                # Source code
    │   ├── blocks/         # Gutenberg blocks
    │   ├── assets/         # Images, fonts, etc.
    │   └── styles/         # CSS/SCSS files
    ├── tests/              # Plugin-specific tests
    │   └── test-plugin.php # PHPUnit test example
    ├── your-plugin.php     # Main plugin file
    ├── package.json        # NPM configuration
    ├── webpack.config.js   # Build configuration
    ├── phpunit.xml         # PHPUnit configuration
    └── block.json          # Block metadata
```

## Complete Template Files

Below are all the template files you need to create a fully functional WordPress plugin development environment. Copy these exactly to avoid issues, especially with Podman.

### 0. WP-CLI Passthrough Script (wp.sh)

```bash
#!/bin/bash
# WP-CLI passthrough script for easier command line usage
# Usage: ./wp.sh <any wp-cli command>
# Example: ./wp.sh plugin list
# Example: ./wp.sh user create testuser test@example.com --role=editor

# Detect container runtime
if command -v podman-compose &> /dev/null; then
    COMPOSE_CMD="podman-compose"
elif command -v docker-compose &> /dev/null; then
    COMPOSE_CMD="docker-compose"
else
    echo "Error: Neither docker-compose nor podman-compose found"
    exit 1
fi

# Check if WordPress container is running
if ! $COMPOSE_CMD ps | grep -q "wordpress.*running\|Up"; then
    echo "Error: WordPress container is not running. Run 'make up' first."
    exit 1
fi

# Pass all arguments to wp-cli in the container
$COMPOSE_CMD exec -T wordpress wp "$@" --allow-root
```

### 1. Environment Configuration (.env.example)

```bash
# Project Configuration
PROJECT_NAME=wp_dev
COMPOSE_PROJECT_NAME=wp_dev

# Random suffix for container names to prevent collisions
# Generate with: echo $((RANDOM % 9000 + 1000))
CONTAINER_SUFFIX=1234

# Host User Configuration (for permissions)
HOST_UID=1000
HOST_GID=1000

# Port Configuration (use random ports to prevent conflicts)
# Generate random ports with: shuf -i 8000-9000 -n 1
WP_PORT=8080
PMA_PORT=8081
MAILHOG_PORT=8025

# Database Configuration
DB_NAME=wordpress_dev
DB_USER=wp_user
DB_PASSWORD=wp_pass123
DB_ROOT_PASSWORD=root_pass123

# WordPress Configuration
WP_ADMIN_USER=admin
WP_ADMIN_PASSWORD=admin123
WP_ADMIN_EMAIL=admin@example.com
WP_SITE_TITLE=Plugin Development Site
WP_SITE_URL=http://localhost:8080

# Development Settings
WP_DEBUG=1
WP_DEBUG_LOG=1
WP_DEBUG_DISPLAY=0
```

### 2. Git Ignore File (.gitignore)

```gitignore
# Environment files
.env
.env.local

# WordPress
/wordpress/

# Database
/db_data/

# Node modules
node_modules/
npm-debug.log*
yarn-debug.log*
yarn-error.log*

# Build outputs
your-plugin/build/
your-plugin/dist/

# IDE files
.vscode/
.idea/
*.swp
*.swo
*~

# OS files
.DS_Store
Thumbs.db

# Logs
*.log
wp-content/debug.log

# Temporary files
*.tmp
*.temp
```

### 3. Enhanced Dockerfile with PHPUnit

```dockerfile
FROM wordpress:latest

# Build arguments for user mapping
ARG HOST_UID=1000
ARG HOST_GID=1000

# Install required packages
RUN apt-get update && \
    apt-get install -y \
    cron \
    vim \
    less \
    mariadb-client \
    git \
    unzip \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

# Install Composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

# Install PHPUnit
RUN composer global require phpunit/phpunit:^9.5 && \
    ln -s /root/.composer/vendor/bin/phpunit /usr/local/bin/phpunit

# Install WP-CLI
RUN curl -O https://raw.githubusercontent.com/wp-cli/builds/gh-pages/phar/wp-cli.phar && \
    chmod +x wp-cli.phar && \
    mv wp-cli.phar /usr/local/bin/wp

# Install WordPress test suite
RUN mkdir -p /tmp/wordpress-tests-lib && \
    curl -L https://develop.svn.wordpress.org/trunk/tests/phpunit/includes/ -o /tmp/wordpress-tests.tar.gz || true

# Create wp-cli directory for www-data user
RUN mkdir -p /var/www/.wp-cli && \
    chown www-data:www-data /var/www/.wp-cli

# Configure Apache to use non-privileged port 8080 for Podman compatibility
RUN sed -i 's/Listen 80/Listen 8080/' /etc/apache2/ports.conf && \
    sed -i 's/:80>/:8080>/' /etc/apache2/sites-available/000-default.conf

# Fix permissions for Podman compatibility
RUN usermod -u ${HOST_UID} www-data && \
    groupmod -g ${HOST_GID} www-data && \
    chown -R www-data:www-data /var/www/html

# Set working directory
WORKDIR /var/www/html

# Expose port 8080 instead of 80
EXPOSE 8080

# Verify installations
RUN wp --info --allow-root && \
    phpunit --version
```

### 4. Enhanced Docker Compose (docker-compose.yml)

```yaml
services:
  wordpress:
    build:
      context: .
      args:
        HOST_UID: ${HOST_UID:-1000}
        HOST_GID: ${HOST_GID:-1000}
    container_name: ${PROJECT_NAME:-wp_dev}_wordpress_${CONTAINER_SUFFIX:-1234}
    restart: unless-stopped
    ports:
      - "${WP_PORT:-8080}:8080"
    environment:
      # Database connection
      WORDPRESS_DB_HOST: db:3306
      WORDPRESS_DB_NAME: ${DB_NAME:-wordpress_dev}
      WORDPRESS_DB_USER: ${DB_USER:-wp_user}
      WORDPRESS_DB_PASSWORD: ${DB_PASSWORD:-wp_pass123}
      
      # WordPress configuration
      WORDPRESS_TABLE_PREFIX: wp_
      WORDPRESS_DEBUG: ${WP_DEBUG:-1}
      
      # Additional PHP settings
      WORDPRESS_CONFIG_EXTRA: |
        define('WP_DEBUG', ${WP_DEBUG:-true});
        define('WP_DEBUG_LOG', ${WP_DEBUG_LOG:-true});
        define('WP_DEBUG_DISPLAY', ${WP_DEBUG_DISPLAY:-false});
        define('WP_MEMORY_LIMIT', '256M');
        define('WP_MAX_MEMORY_LIMIT', '512M');
        define('FS_METHOD', 'direct');
        define('DISABLE_WP_CRON', true);
        
        /* Multisite settings (if needed) */
        // define('WP_ALLOW_MULTISITE', true);
        
        /* Mail settings for MailHog */
        define('WORDPRESS_SMTP_HOST', 'mailhog');
        define('WORDPRESS_SMTP_PORT', '1025');
    volumes:
      # WordPress core (named volume for better performance)
      - wordpress_data:/var/www/html:Z
      
      # WordPress content (named volume)
      - wp_content:/var/www/html/wp-content:Z
      
      # Plugin development (bind mount for live editing)
      - ./your-plugin:/var/www/html/wp-content/plugins/your-plugin:Z
      
      # Custom configuration
      - ./config/uploads.ini:/usr/local/etc/php/conf.d/uploads.ini:ro
      - ./config/php.ini:/usr/local/etc/php/conf.d/custom.ini:ro
      
      # Initialization scripts
      - ./scripts:/docker-entrypoint-init.d:ro
    depends_on:
      db:
        condition: service_healthy
    networks:
      - wp_network
    security_opt:
      - label=disable
    user: "${HOST_UID:-33}:${HOST_GID:-33}"

  db:
    image: mysql:8.0
    container_name: ${PROJECT_NAME:-wp_dev}_mysql_${CONTAINER_SUFFIX:-1234}
    restart: unless-stopped
    command: --default-authentication-plugin=mysql_native_password
    environment:
      MYSQL_ROOT_PASSWORD: ${DB_ROOT_PASSWORD:-root_pass123}
      MYSQL_DATABASE: ${DB_NAME:-wordpress_dev}
      MYSQL_USER: ${DB_USER:-wp_user}
      MYSQL_PASSWORD: ${DB_PASSWORD:-wp_pass123}
    volumes:
      - db_data:/var/lib/mysql:Z
    networks:
      - wp_network
    security_opt:
      - label=disable
    healthcheck:
      test: ["CMD", "mysqladmin", "ping", "-h", "localhost"]
      timeout: 20s
      retries: 10

  phpmyadmin:
    image: phpmyadmin:latest
    container_name: ${PROJECT_NAME:-wp_dev}_phpmyadmin_${CONTAINER_SUFFIX:-1234}
    restart: unless-stopped
    ports:
      - "${PMA_PORT:-8081}:80"
    environment:
      PMA_HOST: db
      PMA_USER: ${DB_USER:-wp_user}
      PMA_PASSWORD: ${DB_PASSWORD:-wp_pass123}
      UPLOAD_LIMIT: 100M
    depends_on:
      - db
    networks:
      - wp_network

  mailhog:
    image: mailhog/mailhog:latest
    container_name: ${PROJECT_NAME:-wp_dev}_mailhog_${CONTAINER_SUFFIX:-1234}
    restart: unless-stopped
    ports:
      - "${MAILHOG_PORT:-8025}:8025"
      - "1025:1025"
    networks:
      - wp_network

  playwright:
    image: mcr.microsoft.com/playwright:v1.40.0-focal
    container_name: ${PROJECT_NAME:-wp_dev}_playwright_${CONTAINER_SUFFIX:-1234}
    restart: unless-stopped
    environment:
      - PLAYWRIGHT_BROWSERS_PATH=/ms-playwright
      - NODE_ENV=development
    volumes:
      - ./tests/e2e:/tests:Z
      - ./tests/results:/results:Z
      - ./package.json:/tests/package.json:ro
      - ./playwright.config.js:/tests/playwright.config.js:ro
    working_dir: /tests
    command: tail -f /dev/null  # Keep container running
    networks:
      - wp_network
    depends_on:
      - wordpress
    security_opt:
      - label=disable

volumes:
  wordpress_data:
    name: ${PROJECT_NAME:-wp_dev}_wordpress_data_${CONTAINER_SUFFIX:-1234}
  wp_content:
    name: ${PROJECT_NAME:-wp_dev}_wp_content_${CONTAINER_SUFFIX:-1234}
  db_data:
    name: ${PROJECT_NAME:-wp_dev}_db_data_${CONTAINER_SUFFIX:-1234}

networks:
  wp_network:
    name: ${PROJECT_NAME:-wp_dev}_network_${CONTAINER_SUFFIX:-1234}
    driver: bridge
```

### 5. Enhanced Makefile

```makefile
# Load environment variables
ifneq (,$(wildcard ./.env))
    include .env
    export
endif

# Detect container runtime
DOCKER := $(shell command -v docker 2> /dev/null)
PODMAN := $(shell command -v podman 2> /dev/null)

ifdef PODMAN
    COMPOSE_CMD := podman-compose
    RUNTIME := podman
else ifdef DOCKER
    COMPOSE_CMD := docker-compose
    RUNTIME := docker
else
    $(error Neither docker nor podman is installed)
endif

# Colors for output
YELLOW := \033[1;33m
GREEN := \033[1;32m
RED := \033[1;31m
NC := \033[0m

.PHONY: help
help: ## Show this help message
	@echo "${GREEN}WordPress Plugin Development Environment${NC}"
	@echo "${YELLOW}Runtime: ${RUNTIME}${NC}"
	@echo ""
	@echo "Usage: make [target]"
	@echo ""
	@echo "Targets:"
	@awk 'BEGIN {FS = ":.*##"} /^[a-zA-Z_-]+:.*?##/ { printf "  ${GREEN}%-15s${NC} %s\n", $$1, $$2 }' $(MAKEFILE_LIST)

.PHONY: check-env
check-env: ## Check environment setup
	@echo "${YELLOW}Checking environment...${NC}"
	@test -f .env || (echo "${RED}ERROR: .env file not found. Copy .env.example to .env${NC}" && exit 1)
	@echo "${GREEN}✓ Environment file found${NC}"
	@echo "${GREEN}✓ Using ${RUNTIME}${NC}"

.PHONY: generate-ports
generate-ports: ## Generate random ports and container suffix
	@echo "${YELLOW}Generating random ports and container suffix...${NC}"
	@WP_PORT=$$(shuf -i 8000-9000 -n 1); \
	PMA_PORT=$$(shuf -i 8000-9000 -n 1); \
	MAILHOG_PORT=$$(shuf -i 8000-9000 -n 1); \
	CONTAINER_SUFFIX=$$(echo $$((RANDOM % 9000 + 1000))); \
	echo "WP_PORT=$$WP_PORT"; \
	echo "PMA_PORT=$$PMA_PORT"; \
	echo "MAILHOG_PORT=$$MAILHOG_PORT"; \
	echo "CONTAINER_SUFFIX=$$CONTAINER_SUFFIX"; \
	echo ""; \
	echo "${GREEN}Add these to your .env file to use random ports and prevent conflicts${NC}"

.PHONY: setup
setup: check-env ## Initial project setup
	@echo "${YELLOW}Setting up project structure...${NC}"
	@mkdir -p scripts config your-plugin/src/{blocks,assets,styles}
	@chmod +x scripts/*.sh 2>/dev/null || true
	@if [ ! -f .env ]; then \
		echo "${YELLOW}Generating .env file with random ports...${NC}"; \
		cp .env.example .env; \
		WP_PORT=$$(shuf -i 8000-9000 -n 1); \
		PMA_PORT=$$(shuf -i 8000-9000 -n 1); \
		MAILHOG_PORT=$$(shuf -i 8000-9000 -n 1); \
		CONTAINER_SUFFIX=$$(echo $$((RANDOM % 9000 + 1000))); \
		sed -i "s/WP_PORT=8080/WP_PORT=$$WP_PORT/" .env; \
		sed -i "s/PMA_PORT=8081/PMA_PORT=$$PMA_PORT/" .env; \
		sed -i "s/MAILHOG_PORT=8025/MAILHOG_PORT=$$MAILHOG_PORT/" .env; \
		sed -i "s/CONTAINER_SUFFIX=1234/CONTAINER_SUFFIX=$$CONTAINER_SUFFIX/" .env; \
		echo "${GREEN}✓ Generated .env with random ports: WP=$$WP_PORT, PMA=$$PMA_PORT, Mail=$$MAILHOG_PORT${NC}"; \
	fi
	@echo "${GREEN}✓ Project structure created${NC}"

.PHONY: build
build: check-env ## Build Docker images
	@echo "${YELLOW}Building images...${NC}"
	$(COMPOSE_CMD) build --build-arg HOST_UID=$$(id -u) --build-arg HOST_GID=$$(id -g)
	@echo "${GREEN}✓ Images built${NC}"

.PHONY: up
up: check-env ## Start all services
	@echo "${YELLOW}Starting services...${NC}"
	$(COMPOSE_CMD) up -d
	@echo ""
	@echo "${GREEN}✓ Services started${NC}"
	@echo ""
	@echo "  WordPress:  http://localhost:${WP_PORT:-8080}"
	@echo "  PHPMyAdmin: http://localhost:${PMA_PORT:-8081}"
	@echo "  MailHog:    http://localhost:${MAILHOG_PORT:-8025}"
	@echo ""
	@echo "  Admin login: ${WP_ADMIN_USER:-admin} / ${WP_ADMIN_PASSWORD:-admin123}"

.PHONY: down
down: ## Stop all services
	@echo "${YELLOW}Stopping services...${NC}"
	$(COMPOSE_CMD) down
	@echo "${GREEN}✓ Services stopped${NC}"

.PHONY: restart
restart: down up ## Restart all services

.PHONY: logs
logs: ## View container logs
	$(COMPOSE_CMD) logs -f

.PHONY: logs-wordpress
logs-wordpress: ## View WordPress logs only
	$(COMPOSE_CMD) logs -f wordpress

.PHONY: shell
shell: ## Access WordPress container shell
	$(COMPOSE_CMD) exec -u www-data wordpress bash

.PHONY: shell-root
shell-root: ## Access WordPress container as root
	$(COMPOSE_CMD) exec wordpress bash

.PHONY: db-shell
db-shell: ## Access database shell
	$(COMPOSE_CMD) exec db mysql -u${DB_USER:-wp_user} -p${DB_PASSWORD:-wp_pass123} ${DB_NAME:-wordpress_dev}

.PHONY: install
install: up ## Install WordPress
	@echo "${YELLOW}Installing WordPress...${NC}"
	@sleep 5
	@if [ -f .env ]; then \
		. ./.env; \
		$(COMPOSE_CMD) exec wordpress wp core install \
			--url="http://localhost:$${WP_PORT:-8080}" \
			--title="$${WP_SITE_TITLE:-Plugin Development Site}" \
			--admin_user="$${WP_ADMIN_USER:-admin}" \
			--admin_password="$${WP_ADMIN_PASSWORD:-admin123}" \
			--admin_email="$${WP_ADMIN_EMAIL:-admin@example.com}" \
			--skip-email \
			--allow-root; \
	else \
		echo "${RED}ERROR: .env file not found. Please run 'make setup' first.${NC}"; \
		exit 1; \
	fi
	@echo "${GREEN}✓ WordPress installed${NC}"

.PHONY: install-manual
install-manual: up ## Manual WordPress install with prompts (for Podman compatibility)
	@echo "${YELLOW}Manual WordPress installation...${NC}"
	@echo "${YELLOW}This command works better with Podman when environment variables aren't expanding${NC}"
	@sleep 5
	@read -p "Enter WordPress site URL (default: http://localhost:8080): " url; \
	read -p "Enter site title (default: Plugin Development Site): " title; \
	read -p "Enter admin username (default: admin): " admin_user; \
	read -p "Enter admin password (default: admin123): " admin_pass; \
	read -p "Enter admin email (default: admin@example.com): " admin_email; \
	$(COMPOSE_CMD) exec wordpress wp core install \
		--url="$${url:-http://localhost:8080}" \
		--title="$${title:-Plugin Development Site}" \
		--admin_user="$${admin_user:-admin}" \
		--admin_password="$${admin_pass:-admin123}" \
		--admin_email="$${admin_email:-admin@example.com}" \
		--skip-email \
		--allow-root
	@echo "${GREEN}✓ WordPress installed${NC}"

.PHONY: wp
wp: ## Run WP-CLI command (usage: make wp ARGS="plugin list")
	$(COMPOSE_CMD) exec wordpress wp $(ARGS) --allow-root

.PHONY: plugin-dev
plugin-dev: ## Start plugin development mode
	@echo "${YELLOW}Starting plugin development...${NC}"
	cd your-plugin && npm start

.PHONY: plugin-build
plugin-build: ## Build plugin for production
	@echo "${YELLOW}Building plugin...${NC}"
	cd your-plugin && npm run build

.PHONY: plugin-install
plugin-install: ## Install plugin dependencies
	@echo "${YELLOW}Installing plugin dependencies...${NC}"
	cd your-plugin && npm install

.PHONY: plugin-activate
plugin-activate: ## Activate the plugin
	$(COMPOSE_CMD) exec wordpress wp plugin activate your-plugin --allow-root

.PHONY: permissions-fix
permissions-fix: ## Fix file permissions
	@echo "${YELLOW}Fixing permissions...${NC}"
	$(COMPOSE_CMD) exec wordpress chown -R www-data:www-data /var/www/html/wp-content/plugins/your-plugin
	@echo "${GREEN}✓ Permissions fixed${NC}"

.PHONY: clean
clean: ## Remove all containers and volumes (WARNING: Data loss!)
	@echo "${RED}WARNING: This will delete all data!${NC}"
	@echo "Press Ctrl+C to cancel, or wait 5 seconds to continue..."
	@sleep 5
	$(COMPOSE_CMD) down -v
	@echo "${GREEN}✓ All containers and volumes removed${NC}"

.PHONY: backup
backup: ## Backup database
	@echo "${YELLOW}Backing up database...${NC}"
	@mkdir -p backups
	$(COMPOSE_CMD) exec db mysqldump -u${DB_USER:-wp_user} -p${DB_PASSWORD:-wp_pass123} ${DB_NAME:-wordpress_dev} > backups/backup-$$(date +%Y%m%d-%H%M%S).sql
	@echo "${GREEN}✓ Database backed up to backups/${NC}"

.PHONY: restore
restore: ## Restore database (usage: make restore FILE=backup.sql)
	@test -n "$(FILE)" || (echo "${RED}ERROR: Please specify FILE=backup.sql${NC}" && exit 1)
	@test -f "$(FILE)" || (echo "${RED}ERROR: File $(FILE) not found${NC}" && exit 1)
	@echo "${YELLOW}Restoring database from $(FILE)...${NC}"
	$(COMPOSE_CMD) exec -T db mysql -u${DB_USER:-wp_user} -p${DB_PASSWORD:-wp_pass123} ${DB_NAME:-wordpress_dev} < $(FILE)
	@echo "${GREEN}✓ Database restored${NC}"

# Testing targets
.PHONY: test
test: test-php test-e2e ## Run all tests

.PHONY: test-php
test-php: ## Run PHPUnit tests
	@echo "${YELLOW}Running PHPUnit tests...${NC}"
	$(COMPOSE_CMD) exec wordpress phpunit --configuration /var/www/html/wp-content/plugins/your-plugin/phpunit.xml

.PHONY: test-php-coverage
test-php-coverage: ## Run PHPUnit tests with coverage
	@echo "${YELLOW}Running PHPUnit tests with coverage...${NC}"
	$(COMPOSE_CMD) exec wordpress phpunit --configuration /var/www/html/wp-content/plugins/your-plugin/phpunit.xml --coverage-html coverage

.PHONY: test-e2e
test-e2e: ## Run Playwright E2E tests
	@echo "${YELLOW}Running Playwright E2E tests...${NC}"
	$(COMPOSE_CMD) exec playwright npm test

.PHONY: test-e2e-ui
test-e2e-ui: ## Run Playwright tests with UI
	@echo "${YELLOW}Running Playwright tests with UI...${NC}"
	$(COMPOSE_CMD) exec playwright npx playwright test --ui

.PHONY: test-e2e-debug
test-e2e-debug: ## Debug Playwright tests
	@echo "${YELLOW}Running Playwright tests in debug mode...${NC}"
	$(COMPOSE_CMD) exec playwright npx playwright test --debug

.PHONY: playwright-shell
playwright-shell: ## Access Playwright container shell
	$(COMPOSE_CMD) exec playwright bash

.PHONY: install-e2e-deps
install-e2e-deps: ## Install E2E test dependencies
	@echo "${YELLOW}Installing E2E test dependencies...${NC}"
	$(COMPOSE_CMD) exec playwright npm install
```

### 6. Enhanced WordPress Initialization Script (scripts/init-wordpress.sh)

```bash
#!/bin/bash
set -e

echo "=== WordPress Initialization Script ==="

# Wait for WordPress to be ready
echo "Waiting for WordPress to be ready..."
until wp core version --allow-root > /dev/null 2>&1; do
    echo -n "."
    sleep 2
done
echo ""

# Check if WordPress is already installed
if wp core is-installed --allow-root 2>/dev/null; then
    echo "WordPress is already installed. Skipping installation."
else
    echo "Installing WordPress..."
    wp core install \
        --url="${WP_SITE_URL:-http://localhost:8080}" \
        --title="${WP_SITE_TITLE:-Plugin Development Site}" \
        --admin_user="${WP_ADMIN_USER:-admin}" \
        --admin_password="${WP_ADMIN_PASSWORD:-admin123}" \
        --admin_email="${WP_ADMIN_EMAIL:-admin@example.com}" \
        --skip-email \
        --allow-root
fi

# Update WordPress options
echo "Configuring WordPress settings..."
wp option update siteurl "${WP_SITE_URL:-http://localhost:8080}" --allow-root
wp option update home "${WP_SITE_URL:-http://localhost:8080}" --allow-root
wp option update blogdescription "WordPress Plugin Development Environment" --allow-root
wp option update timezone_string "America/New_York" --allow-root
wp option update start_of_week 1 --allow-root
wp option update default_comment_status closed --allow-root
wp option update default_ping_status closed --allow-root

# Set permalink structure
wp rewrite structure '/%postname%/' --allow-root

# Configure media settings
wp option update thumbnail_size_w 300 --allow-root
wp option update thumbnail_size_h 300 --allow-root
wp option update medium_size_w 600 --allow-root
wp option update medium_size_h 600 --allow-root
wp option update large_size_w 1200 --allow-root
wp option update large_size_h 1200 --allow-root

# Install and activate useful development plugins
echo "Installing development plugins..."
plugins=(
    "query-monitor"
    "debug-bar"
    "regenerate-thumbnails"
    "classic-editor"
    "show-current-template"
    "wp-mail-smtp"
)

for plugin in "${plugins[@]}"; do
    if ! wp plugin is-installed "$plugin" --allow-root 2>/dev/null; then
        echo "Installing $plugin..."
        wp plugin install "$plugin" --allow-root || true
    fi
    
    if wp plugin is-installed "$plugin" --allow-root 2>/dev/null && ! wp plugin is-active "$plugin" --allow-root 2>/dev/null; then
        echo "Activating $plugin..."
        wp plugin activate "$plugin" --allow-root || true
    fi
done

# Configure WP Mail SMTP for MailHog
if wp plugin is-active "wp-mail-smtp" --allow-root 2>/dev/null; then
    echo "Configuring WP Mail SMTP for MailHog..."
    wp option update wp_mail_smtp '{"mail":{"from_email":"wordpress@localhost","from_name":"WordPress Dev","mailer":"smtp","return_path":false},"smtp":{"host":"mailhog","port":1025,"encryption":"none","auth":false,"autotls":false}}' --format=json --allow-root || true
fi

# Activate your plugin if it exists
if [ -d "/var/www/html/wp-content/plugins/your-plugin" ]; then
    echo "Activating your-plugin..."
    wp plugin activate your-plugin --allow-root || true
fi

# Create uploads directory with correct permissions
echo "Setting up uploads directory..."
mkdir -p /var/www/html/wp-content/uploads
chown -R www-data:www-data /var/www/html/wp-content/uploads
chmod -R 755 /var/www/html/wp-content/uploads

# Create basic pages
echo "Creating default pages..."
pages=("Home" "About" "Services" "Contact")
for page in "${pages[@]}"; do
    if ! wp post list --post_type=page --title="$page" --format=count --allow-root | grep -q "1"; then
        wp post create --post_type=page --post_title="$page" --post_status=publish --allow-root
    fi
done

# Set homepage
home_id=$(wp post list --post_type=page --title="Home" --format=ids --allow-root)
if [ -n "$home_id" ]; then
    wp option update page_on_front "$home_id" --allow-root
    wp option update show_on_front page --allow-root
fi

echo "=== WordPress initialization complete! ==="
echo "URL: ${WP_SITE_URL:-http://localhost:8080}"
echo "Admin: ${WP_ADMIN_USER:-admin} / ${WP_ADMIN_PASSWORD:-admin123}"
```

### 7. WP-Cron Configuration Script (scripts/wp-cron.sh)

```bash
#!/bin/bash
set -e

echo "=== Configuring WordPress Cron ==="

# Wait for WordPress to be available
until wp core version --allow-root > /dev/null 2>&1; do
    sleep 2
done

# Check if DISABLE_WP_CRON is already set
if ! grep -q "DISABLE_WP_CRON" /var/www/html/wp-config.php; then
    echo "Disabling WordPress pseudo-cron..."
    wp config set DISABLE_WP_CRON true --raw --allow-root
fi

# Set up system cron job
echo "Setting up system cron..."
CRON_JOB="*/5 * * * * cd /var/www/html && /usr/local/bin/wp cron event run --due-now --allow-root >> /var/log/wp-cron.log 2>&1"

# Add cron job if it doesn't exist
if ! crontab -l 2>/dev/null | grep -q "wp cron event run"; then
    (crontab -l 2>/dev/null; echo "$CRON_JOB") | crontab -
fi

# Create log file
touch /var/log/wp-cron.log
chown www-data:www-data /var/log/wp-cron.log

# Start cron service
service cron start

echo "=== WordPress cron configured successfully ==="
```

### 8. Permissions Fix Script for Podman (scripts/fix-permissions.sh)

```bash
#!/bin/bash
set -e

echo "=== Fixing file permissions ==="

# Get the user ID from environment or default to www-data
USER_ID=${HOST_UID:-33}
GROUP_ID=${HOST_GID:-33}

# Fix ownership of WordPress files
if [ -d "/var/www/html" ]; then
    echo "Setting ownership of WordPress files..."
    chown -R ${USER_ID}:${GROUP_ID} /var/www/html
fi

# Ensure plugin directory has correct permissions
if [ -d "/var/www/html/wp-content/plugins/your-plugin" ]; then
    echo "Setting plugin directory permissions..."
    chown -R ${USER_ID}:${GROUP_ID} /var/www/html/wp-content/plugins/your-plugin
    find /var/www/html/wp-content/plugins/your-plugin -type d -exec chmod 755 {} \;
    find /var/www/html/wp-content/plugins/your-plugin -type f -exec chmod 644 {} \;
fi

# Ensure uploads directory exists and has correct permissions
echo "Setting uploads directory permissions..."
mkdir -p /var/www/html/wp-content/uploads
chown -R ${USER_ID}:${GROUP_ID} /var/www/html/wp-content/uploads
chmod -R 755 /var/www/html/wp-content/uploads

echo "=== Permissions fixed ==="
```

### 9. SVG Upload Enabler Script (scripts/enable-svg.sh)

```bash
#!/bin/bash
set -e

echo "=== Enabling SVG uploads ==="

# Create mu-plugins directory
mkdir -p /var/www/html/wp-content/mu-plugins

# Create SVG enabler plugin
cat > /var/www/html/wp-content/mu-plugins/enable-svg.php << 'EOF'
<?php
/**
 * Enable SVG uploads
 */
add_filter('upload_mimes', function($mimes) {
    $mimes['svg'] = 'image/svg+xml';
    $mimes['svgz'] = 'image/svg+xml';
    return $mimes;
});

add_filter('wp_check_filetype_and_ext', function($data, $file, $filename, $mimes) {
    $filetype = wp_check_filetype($filename, $mimes);
    return [
        'ext' => $filetype['ext'],
        'type' => $filetype['type'],
        'proper_filename' => $data['proper_filename']
    ];
}, 10, 4);

add_action('admin_head', function() {
    echo '<style>
        .attachment-info .thumbnail img[src$=".svg"],
        .media-icon img[src$=".svg"] {
            width: 100% !important;
            height: auto !important;
        }
    </style>';
});
EOF

chown www-data:www-data /var/www/html/wp-content/mu-plugins/enable-svg.php

echo "=== SVG uploads enabled ==="
```

### 10. Page Creation Script (scripts/create-pages.sh)

```bash
#!/bin/bash
set -e

echo "=== Creating default pages with content ==="

# Wait for WordPress
until wp core version --allow-root > /dev/null 2>&1; do
    sleep 2
done

# Create home page with Gutenberg blocks
wp post create \
    --post_type=page \
    --post_title="Home" \
    --post_status=publish \
    --post_content='<!-- wp:your-plugin/hero-banner -->
<div class="wp-block-your-plugin-hero-banner">
    <h1>Welcome to Our Site</h1>
    <p>This is a hero banner block from your custom plugin.</p>
</div>
<!-- /wp:your-plugin/hero-banner -->

<!-- wp:paragraph -->
<p>Add your content here.</p>
<!-- /wp:paragraph -->' \
    --allow-root || echo "Home page already exists"

# Create other pages
pages=(
    "About:Learn more about our company and mission."
    "Services:Discover our professional services."
    "Contact:Get in touch with our team."
)

for page_data in "${pages[@]}"; do
    IFS=':' read -r title content <<< "$page_data"
    
    if ! wp post list --post_type=page --title="$title" --format=count --allow-root | grep -q "1"; then
        wp post create \
            --post_type=page \
            --post_title="$title" \
            --post_status=publish \
            --post_content="<!-- wp:paragraph --><p>$content</p><!-- /wp:paragraph -->" \
            --allow-root
    fi
done

echo "=== Pages created successfully ==="
```

### 11. PHP Configuration (config/php.ini)

```ini
; Development PHP configuration
display_errors = On
display_startup_errors = On
error_reporting = E_ALL
log_errors = On
error_log = /var/www/html/wp-content/debug.log

; Performance settings
memory_limit = 256M
max_execution_time = 300
max_input_time = 300
max_input_vars = 3000

; File upload settings
file_uploads = On
upload_max_filesize = 100M
post_max_size = 100M
max_file_uploads = 20

; Session settings
session.gc_maxlifetime = 1440
session.save_path = "/tmp"

; OPcache settings (for better performance)
opcache.enable = 1
opcache.memory_consumption = 128
opcache.interned_strings_buffer = 8
opcache.max_accelerated_files = 4000
opcache.revalidate_freq = 0
opcache.fast_shutdown = 1
```

### 12. Upload Configuration (config/uploads.ini)

```ini
; Upload size limits
upload_max_filesize = 100M
post_max_size = 100M
max_execution_time = 300
max_input_time = 300
memory_limit = 256M
```

### 13. Plugin Starter Files

#### Main Plugin File (your-plugin/your-plugin.php)

```php
<?php
/**
 * Plugin Name: Your Plugin Name
 * Plugin URI: https://your-website.com/
 * Description: A custom WordPress plugin with Gutenberg blocks
 * Version: 1.0.0
 * Author: Your Name
 * Author URI: https://your-website.com/
 * License: GPL v2 or later
 * Text Domain: your-plugin
 * Domain Path: /languages
 */

// Prevent direct access
if (!defined('ABSPATH')) {
    exit;
}

// Define plugin constants
define('YOUR_PLUGIN_VERSION', '1.0.0');
define('YOUR_PLUGIN_URL', plugin_dir_url(__FILE__));
define('YOUR_PLUGIN_PATH', plugin_dir_path(__FILE__));

// Plugin activation hook
register_activation_hook(__FILE__, 'your_plugin_activate');
function your_plugin_activate() {
    // Activation tasks
    flush_rewrite_rules();
}

// Plugin deactivation hook
register_deactivation_hook(__FILE__, 'your_plugin_deactivate');
function your_plugin_deactivate() {
    // Deactivation tasks
    flush_rewrite_rules();
}

// Load plugin textdomain
add_action('plugins_loaded', 'your_plugin_load_textdomain');
function your_plugin_load_textdomain() {
    load_plugin_textdomain('your-plugin', false, dirname(plugin_basename(__FILE__)) . '/languages');
}

// Register block assets
add_action('init', 'your_plugin_register_blocks');
function your_plugin_register_blocks() {
    // Register block editor assets
    wp_register_script(
        'your-plugin-blocks',
        YOUR_PLUGIN_URL . 'build/index.js',
        array('wp-blocks', 'wp-element', 'wp-editor', 'wp-components', 'wp-i18n'),
        YOUR_PLUGIN_VERSION,
        true
    );

    // Register block editor styles
    wp_register_style(
        'your-plugin-blocks-editor',
        YOUR_PLUGIN_URL . 'build/editor.css',
        array('wp-edit-blocks'),
        YOUR_PLUGIN_VERSION
    );

    // Register front-end styles
    wp_register_style(
        'your-plugin-blocks-frontend',
        YOUR_PLUGIN_URL . 'build/style.css',
        array(),
        YOUR_PLUGIN_VERSION
    );

    // Register blocks
    register_block_type('your-plugin/example-block', array(
        'editor_script' => 'your-plugin-blocks',
        'editor_style' => 'your-plugin-blocks-editor',
        'style' => 'your-plugin-blocks-frontend',
    ));
}

// Add custom block category
add_filter('block_categories_all', 'your_plugin_block_categories', 10, 2);
function your_plugin_block_categories($categories, $post) {
    return array_merge(
        array(
            array(
                'slug' => 'your-plugin',
                'title' => __('Your Plugin Blocks', 'your-plugin'),
                'icon' => 'wordpress',
            ),
        ),
        $categories
    );
}
```

#### Package.json (your-plugin/package.json)

```json
{
  "name": "your-plugin",
  "version": "1.0.0",
  "description": "Custom WordPress plugin with Gutenberg blocks",
  "author": "Your Name",
  "license": "GPL-2.0-or-later",
  "main": "build/index.js",
  "scripts": {
    "build": "wp-scripts build",
    "start": "wp-scripts start",
    "lint:js": "wp-scripts lint-js",
    "lint:css": "wp-scripts lint-style",
    "lint": "npm run lint:js && npm run lint:css",
    "format": "wp-scripts format",
    "packages-update": "wp-scripts packages-update"
  },
  "devDependencies": {
    "@wordpress/scripts": "^26.0.0",
    "@wordpress/env": "^8.0.0"
  },
  "dependencies": {
    "@wordpress/block-editor": "^12.0.0",
    "@wordpress/blocks": "^12.0.0",
    "@wordpress/components": "^25.0.0",
    "@wordpress/element": "^5.0.0",
    "@wordpress/i18n": "^4.0.0"
  }
}
```

#### Webpack Configuration (your-plugin/webpack.config.js)

```javascript
const defaultConfig = require('@wordpress/scripts/config/webpack.config');
const path = require('path');

module.exports = {
    ...defaultConfig,
    entry: {
        index: path.resolve(__dirname, 'src/index.js'),
        // Add more entry points for different blocks if needed
    },
    output: {
        ...defaultConfig.output,
        path: path.resolve(__dirname, 'build'),
    },
};
```

#### Block Registration (your-plugin/src/index.js)

```javascript
/**
 * WordPress dependencies
 */
import { registerBlockType } from '@wordpress/blocks';
import { __ } from '@wordpress/i18n';

/**
 * Internal dependencies
 */
import './editor.scss';
import './style.scss';

// Import block definitions
import * as exampleBlock from './blocks/example-block';

// Register blocks
const blocks = [
    exampleBlock,
];

blocks.forEach((block) => {
    if (!block) {
        return;
    }
    
    const { name, settings } = block;
    registerBlockType(name, settings);
});
```

#### Example Block (your-plugin/src/blocks/example-block/index.js)

```javascript
/**
 * WordPress dependencies
 */
import { __ } from '@wordpress/i18n';
import { useBlockProps } from '@wordpress/block-editor';

/**
 * Block name
 */
export const name = 'your-plugin/example-block';

/**
 * Block settings
 */
export const settings = {
    title: __('Example Block', 'your-plugin'),
    description: __('An example custom block', 'your-plugin'),
    category: 'your-plugin',
    icon: 'smiley',
    supports: {
        html: false,
        align: ['wide', 'full'],
    },
    attributes: {
        content: {
            type: 'string',
            default: '',
        },
    },
    edit: ({ attributes, setAttributes }) => {
        const blockProps = useBlockProps();
        
        return (
            <div {...blockProps}>
                <input
                    type="text"
                    value={attributes.content}
                    onChange={(e) => setAttributes({ content: e.target.value })}
                    placeholder={__('Enter content...', 'your-plugin')}
                />
            </div>
        );
    },
    save: ({ attributes }) => {
        const blockProps = useBlockProps.save();
        
        return (
            <div {...blockProps}>
                <p>{attributes.content}</p>
            </div>
        );
    },
};
```

#### Editor Styles (your-plugin/src/editor.scss)

```scss
/**
 * Editor styles for all blocks
 */

// Import WordPress base styles
@import "~@wordpress/base-styles/colors";
@import "~@wordpress/base-styles/breakpoints";
@import "~@wordpress/base-styles/mixins";

// Example block editor styles
.wp-block-your-plugin-example-block {
    padding: 20px;
    border: 2px dashed #ddd;
    
    input {
        width: 100%;
        padding: 10px;
        font-size: 16px;
        border: 1px solid #ccc;
        border-radius: 4px;
    }
}
```

#### Frontend Styles (your-plugin/src/style.scss)

```scss
/**
 * Frontend styles for all blocks
 */

// Example block frontend styles
.wp-block-your-plugin-example-block {
    padding: 20px;
    background-color: #f5f5f5;
    border-radius: 4px;
    
    p {
        margin: 0;
        font-size: 18px;
        color: #333;
    }
}
```

### 14. Testing Configuration Files

#### PHPUnit Configuration (your-plugin/phpunit.xml)

```xml
<?xml version="1.0" encoding="UTF-8"?>
<phpunit
    bootstrap="tests/bootstrap.php"
    colors="true"
    convertErrorsToExceptions="true"
    convertNoticesToExceptions="true"
    convertWarningsToExceptions="true"
    processIsolation="false"
    stopOnFailure="false">
    
    <testsuites>
        <testsuite name="Plugin Test Suite">
            <directory suffix="-test.php">./tests</directory>
        </testsuite>
    </testsuites>
    
    <filter>
        <whitelist processUncoveredFilesFromWhitelist="true">
            <directory suffix=".php">./</directory>
            <exclude>
                <directory>./tests</directory>
                <directory>./vendor</directory>
                <directory>./node_modules</directory>
                <directory>./build</directory>
            </exclude>
        </whitelist>
    </filter>
</phpunit>
```

#### PHPUnit Bootstrap (your-plugin/tests/bootstrap.php)

```php
<?php
/**
 * PHPUnit bootstrap file for plugin tests
 */

// Define test constants
define('WP_TESTS_PHPUNIT_POLYFILLS_PATH', '/tmp/wordpress-tests-lib/includes/phpunit-polyfills');

// Load WordPress test environment
$_tests_dir = getenv('WP_TESTS_DIR');
if (!$_tests_dir) {
    $_tests_dir = '/tmp/wordpress-tests-lib';
}

// Give access to tests_add_filter() function
require_once $_tests_dir . '/includes/functions.php';

// Load the plugin
function _manually_load_plugin() {
    require dirname(dirname(__FILE__)) . '/your-plugin.php';
}
tests_add_filter('muplugins_loaded', '_manually_load_plugin');

// Start up the WP testing environment
require $_tests_dir . '/includes/bootstrap.php';
```

#### Example PHPUnit Test (your-plugin/tests/test-plugin.php)

```php
<?php
/**
 * Class Test_Plugin
 *
 * @package Your_Plugin
 */

class Test_Plugin extends WP_UnitTestCase {
    
    /**
     * Test plugin activation
     */
    public function test_plugin_activated() {
        $this->assertTrue(is_plugin_active('your-plugin/your-plugin.php'));
    }
    
    /**
     * Test plugin constants
     */
    public function test_plugin_constants() {
        $this->assertTrue(defined('YOUR_PLUGIN_VERSION'));
        $this->assertTrue(defined('YOUR_PLUGIN_URL'));
        $this->assertTrue(defined('YOUR_PLUGIN_PATH'));
    }
    
    /**
     * Test block registration
     */
    public function test_block_registered() {
        $registry = WP_Block_Type_Registry::get_instance();
        $this->assertTrue($registry->is_registered('your-plugin/example-block'));
    }
    
    /**
     * Test custom block category
     */
    public function test_block_category() {
        $categories = apply_filters('block_categories_all', array(), null);
        $category_slugs = wp_list_pluck($categories, 'slug');
        $this->assertContains('your-plugin', $category_slugs);
    }
}
```

### 15. E2E Testing Configuration

#### Playwright Configuration (playwright.config.js)

```javascript
// @ts-check
const { defineConfig, devices } = require('@playwright/test');

module.exports = defineConfig({
  testDir: './tests/e2e',
  timeout: 30 * 1000,
  expect: {
    timeout: 5000
  },
  fullyParallel: true,
  forbidOnly: !!process.env.CI,
  retries: process.env.CI ? 2 : 0,
  workers: process.env.CI ? 1 : undefined,
  reporter: 'html',
  use: {
    baseURL: process.env.WP_BASE_URL || 'http://wordpress',
    trace: 'on-first-retry',
    screenshot: 'only-on-failure',
  },

  projects: [
    {
      name: 'chromium',
      use: { ...devices['Desktop Chrome'] },
    },
    {
      name: 'firefox',
      use: { ...devices['Desktop Firefox'] },
    },
    {
      name: 'webkit',
      use: { ...devices['Desktop Safari'] },
    },
  ],

  webServer: {
    command: 'echo "WordPress is already running in container"',
    port: 80,
    reuseExistingServer: true,
  },
});
```

#### Example E2E Test (tests/e2e/example.spec.js)

```javascript
// @ts-check
const { test, expect } = require('@playwright/test');

test.describe('WordPress Plugin E2E Tests', () => {
  test.beforeEach(async ({ page }) => {
    // Login to WordPress admin
    await page.goto('/wp-login.php');
    await page.fill('#user_login', 'admin');
    await page.fill('#user_pass', 'admin123');
    await page.click('#wp-submit');
    await expect(page).toHaveURL(/wp-admin/);
  });

  test('plugin is activated', async ({ page }) => {
    await page.goto('/wp-admin/plugins.php');
    
    // Check if plugin is in the list and activated
    const pluginRow = page.locator('tr[data-slug="your-plugin"]');
    await expect(pluginRow).toBeVisible();
    
    const deactivateLink = pluginRow.locator('a:has-text("Deactivate")');
    await expect(deactivateLink).toBeVisible();
  });

  test('custom block appears in block editor', async ({ page }) => {
    // Create new post
    await page.goto('/wp-admin/post-new.php');
    
    // Open block inserter
    await page.click('button[aria-label="Toggle block inserter"]');
    
    // Search for our custom block
    await page.fill('input[placeholder="Search"]', 'Example Block');
    
    // Check if block appears
    const block = page.locator('button:has-text("Example Block")');
    await expect(block).toBeVisible();
    
    // Insert block
    await block.click();
    
    // Verify block is inserted
    const insertedBlock = page.locator('.wp-block-your-plugin-example-block');
    await expect(insertedBlock).toBeVisible();
  });

  test('block saves content correctly', async ({ page }) => {
    await page.goto('/wp-admin/post-new.php');
    
    // Add title
    await page.fill('[aria-label="Add title"]', 'Test Post with Custom Block');
    
    // Insert custom block
    await page.click('button[aria-label="Toggle block inserter"]');
    await page.fill('input[placeholder="Search"]', 'Example Block');
    await page.click('button:has-text("Example Block")');
    
    // Add content to block
    await page.fill('.wp-block-your-plugin-example-block input', 'Test content');
    
    // Publish post
    await page.click('button:has-text("Publish")');
    await page.click('button:has-text("Publish"):visible'); // Confirm
    
    // View post
    await page.click('a:has-text("View Post")');
    
    // Verify content on frontend
    const blockContent = page.locator('.wp-block-your-plugin-example-block p');
    await expect(blockContent).toHaveText('Test content');
  });
});
```

#### E2E Test Package.json (tests/e2e/package.json)

```json
{
  "name": "your-plugin-e2e-tests",
  "version": "1.0.0",
  "description": "E2E tests for Your Plugin",
  "scripts": {
    "test": "playwright test",
    "test:ui": "playwright test --ui",
    "test:debug": "playwright test --debug",
    "test:report": "playwright show-report"
  },
  "devDependencies": {
    "@playwright/test": "^1.40.0"
  }
}
```


## User Permissions and File Ownership

### Understanding Container User Context

WordPress containers run as `www-data` (UID 33, GID 33) by default. This can cause permission issues with bind-mounted volumes.

### Solutions for Permission Management

#### Option 1: Match Host User to Container User (Recommended)

Create a custom Dockerfile that sets the correct user:

```dockerfile
FROM wordpress:latest

# Get host user ID from build argument
ARG HOST_UID=1000
ARG HOST_GID=1000

# Create user matching host system
RUN groupmod -g $HOST_GID www-data && \
    usermod -u $HOST_UID www-data && \
    chown -R www-data:www-data /var/www/html
```

Build with your user ID:

```bash
docker build --build-arg HOST_UID=$(id -u) --build-arg HOST_GID=$(id -g) -t wordpress-dev .
```

#### Option 2: Use Init Script for Permissions

Create `scripts/fix-permissions.sh`:

```bash
#!/bin/bash
# Fix permissions for plugin directory
chown -R www-data:www-data /var/www/html/wp-content/plugins/your-plugin
chmod -R 755 /var/www/html/wp-content/plugins/your-plugin
find /var/www/html/wp-content/plugins/your-plugin -type f -exec chmod 644 {} \;
```

#### Option 3: Run Container as Host User

Add to docker-compose.yml service:

```yaml
user: "${UID}:${GID}"
```

Then run with:

```bash
UID=$(id -u) GID=$(id -g) docker-compose up
```

## Volume Mapping Strategy

### Named Volumes vs Bind Mounts

#### Use Named Volumes For:
- WordPress core files (`wordpress_data`)
- WordPress content (`wp_content`)
- Database data (`db_data`)
- Better performance and isolation

#### Use Bind Mounts For:
- Active plugin development directory
- Configuration files
- Scripts that need editing

### Volume Configuration Best Practices

1. **SELinux Labels** (`:Z` suffix):
   - Use for Podman/SELinux systems
   - Provides proper security context

2. **Read-Only Mounts** (`:ro` suffix):
   - Use for initialization scripts
   - Prevents accidental modifications

3. **Plugin Development Mount**:
   ```yaml
   - ./your-plugin:/var/www/html/wp-content/plugins/your-plugin:Z
   ```
   - Enables hot-reload during development
   - Changes reflect immediately

## Step-by-Step Setup

### 1. Create Project Structure

```bash
mkdir wordpress-plugin-dev
cd wordpress-plugin-dev
mkdir scripts
mkdir your-plugin
```

### 2. Create Initialization Scripts

Copy the initialization scripts from the template files above:
- Enhanced WordPress Initialization Script (scripts/init-wordpress.sh) - See template #6
- WP-Cron Configuration Script (scripts/wp-cron.sh) - See template #7
- Permissions Fix Script for Podman (scripts/fix-permissions.sh) - See template #8
- SVG Upload Enabler Script (scripts/enable-svg.sh) - See template #9
- Page Creation Script (scripts/create-pages.sh) - See template #10

### 3. Create Makefile

Copy the Enhanced Makefile from template #5 above.

### 4. Initialize Plugin Structure

Copy the plugin starter files from the templates above:
- Main Plugin File (your-plugin/your-plugin.php) - See template #13
- Package.json (your-plugin/package.json) - See template #13
- Webpack Configuration (your-plugin/webpack.config.js) - See template #13
- Block Registration (your-plugin/src/index.js) - See template #13
- Example Block (your-plugin/src/blocks/example-block/index.js) - See template #13
- Editor Styles (your-plugin/src/editor.scss) - See template #13
- Frontend Styles (your-plugin/src/style.scss) - See template #13

Then install dependencies:
```bash
cd your-plugin
npm install
```

### 5. Set Correct Permissions

```bash
# Make scripts executable
chmod +x scripts/*.sh
chmod +x wp.sh

# Set initial permissions for plugin directory
chmod -R 755 your-plugin

# Create test directories
mkdir -p tests/e2e tests/phpunit tests/results
```

### 6. Start Development Environment

```bash
# Option 1: Use make setup to auto-generate .env with random ports
make setup

# Option 2: Manual setup - Copy .env.example to .env and edit as needed
cp .env.example .env

# Generate random ports to prevent conflicts (optional)
make generate-ports

# Edit .env to set your user ID for Podman compatibility
echo "HOST_UID=$(id -u)" >> .env
echo "HOST_GID=$(id -g)" >> .env

# Build and start containers
make build
make up

# Run initial WordPress installation
make install

# Install plugin dependencies
make plugin-install

# Start plugin development
make plugin-dev
```

### 7. Verify Everything is Working

```bash
# Check container status
docker ps  # or podman ps

# View logs if needed
make logs

# Access WordPress admin
# http://localhost:8080/wp-admin
# Username: admin
# Password: admin123

# Check email capture
# http://localhost:8025
```

## Development Workflow

### Daily Development

1. **Start Environment**:
   ```bash
   make up
   ```

2. **Start Plugin Development Mode**:
   ```bash
   make plugin-dev
   ```

3. **Access WordPress**:
   - Frontend: http://localhost:8080
   - Admin: http://localhost:8080/wp-admin
   - PHPMyAdmin: http://localhost:8081
   - MailHog: http://localhost:8025
   - Login: admin / admin123

4. **Make Plugin Changes**:
   - Edit files in `your-plugin/src/`
   - Changes auto-compile with webpack
   - Refresh browser to see changes

5. **Run WP-CLI Commands**:
   ```bash
   # Using make
   make wp ARGS='plugin list'
   make wp ARGS='option get siteurl'
   
   # Using wp.sh passthrough script
   ./wp.sh plugin list
   ./wp.sh user create testuser test@example.com --role=editor
   ./wp.sh db export backup.sql
   ```

6. **Access Container Shell**:
   ```bash
   make shell
   ```

7. **Stop Environment**:
   ```bash
   make down
   ```

### Testing Workflow

#### Running PHPUnit Tests

1. **Run all PHP tests**:
   ```bash
   make test-php
   ```

2. **Run tests with coverage**:
   ```bash
   make test-php-coverage
   # Coverage report will be in your-plugin/coverage/
   ```

3. **Run specific test file**:
   ```bash
   ./wp.sh eval "cd wp-content/plugins/your-plugin && phpunit tests/test-specific.php"
   ```

#### Running E2E Tests with Playwright

1. **Install E2E dependencies** (first time):
   ```bash
   make install-e2e-deps
   ```

2. **Run all E2E tests**:
   ```bash
   make test-e2e
   ```

3. **Run tests with UI mode** (interactive):
   ```bash
   make test-e2e-ui
   ```

4. **Debug failing tests**:
   ```bash
   make test-e2e-debug
   ```

5. **Access Playwright container**:
   ```bash
   make playwright-shell
   ```

6. **View test reports**:
   ```bash
   # Reports are saved in tests/results/
   ```

#### Running All Tests

```bash
make test  # Runs both PHPUnit and Playwright tests
```

### Building for Production

```bash
make plugin-build
```

This creates optimized assets in `your-plugin/build/`.

## Troubleshooting

### Permission Issues

**Problem**: Can't write files in plugin directory

**Solutions**:

1. **Fix ownership from host**:
   ```bash
   sudo chown -R $(id -u):$(id -g) your-plugin/
   ```

2. **Fix permissions in container**:
   ```bash
   make shell
   chown -R www-data:www-data /var/www/html/wp-content/plugins/your-plugin
   ```

3. **Use Docker user mapping**:
   ```bash
   # Add to .env file
   UID=1000
   GID=1000
   ```

### Database Connection Issues

**Problem**: WordPress can't connect to database

**Solutions**:

1. **Check container status**:
   ```bash
   docker ps
   make logs
   ```

2. **Verify database credentials** in docker-compose.yml

3. **Wait for database to be ready**:
   ```bash
   make down
   make up
   sleep 10
   make install
   ```

### Plugin Not Appearing

**Problem**: Plugin doesn't show in WordPress admin

**Solutions**:

1. **Check plugin file structure**:
   - Main plugin file must be in root of plugin directory
   - Plugin header must be valid

2. **Check file permissions**:
   ```bash
   make shell
   ls -la /var/www/html/wp-content/plugins/
   ```

3. **Check for PHP errors**:
   ```bash
   make wp ARGS='plugin activate your-plugin --debug'
   ```

### Port Conflicts

**Problem**: Ports 8080/8081 already in use

**Solutions**:

1. **Use random port generation** (Recommended):
   ```bash
   make generate-ports
   # Copy the output to your .env file
   ```

2. **Manual port change** in .env file:
   ```bash
   WP_PORT=8888
   PMA_PORT=8889
   MAILHOG_PORT=8890
   ```

3. **Change ports in docker-compose.yml**:
   ```yaml
   ports:
     - "8888:80"  # Change from 8080
   ```

## Security Considerations

### Development Only Settings

⚠️ **WARNING**: This setup is for development only. Never use in production!

**Insecure elements**:
- Debug mode enabled
- Weak passwords
- Direct file system access
- SELinux disabled
- No SSL/TLS

### Production Considerations

For production WordPress:

1. **Use strong passwords**
2. **Enable SSL/TLS**
3. **Disable debug mode**
4. **Use proper file permissions**
5. **Enable SELinux**
6. **Use managed database**
7. **Implement backup strategy**
8. **Use security plugins**
9. **Keep WordPress updated**
10. **Use environment variables for secrets**

### Secure Development Practices

1. **Never commit sensitive data**:
   - Add `.env` to `.gitignore`
   - Use environment variables for credentials

2. **Regular updates**:
   ```bash
   docker pull wordpress:latest
   docker pull mysql:8.0
   make build
   ```

3. **Use read-only mounts** where possible

4. **Limit container capabilities**:
   ```yaml
   cap_drop:
     - ALL
   cap_add:
     - CHOWN
     - SETUID
     - SETGID
   ```

## Advanced Configuration

### Custom PHP Configuration

PHP configuration files are provided in the templates:
- PHP Configuration (config/php.ini) - See template #11
- Upload Configuration (config/uploads.ini) - See template #12

These are automatically mounted via docker-compose.yml.

### Xdebug for PHP Debugging

Add to Dockerfile:
```dockerfile
RUN pecl install xdebug && docker-php-ext-enable xdebug
```

Create `config/xdebug.ini`:
```ini
xdebug.mode=develop,debug
xdebug.client_host=host.docker.internal
xdebug.start_with_request=yes
xdebug.idekey=VSCODE
```

Add to docker-compose.yml volumes:
```yaml
- ./config/xdebug.ini:/usr/local/etc/php/conf.d/xdebug.ini:ro
```

### Multiple Plugin Development

```yaml
volumes:
  - ./plugin-one:/var/www/html/wp-content/plugins/plugin-one:Z
  - ./plugin-two:/var/www/html/wp-content/plugins/plugin-two:Z
```

### Using with VS Code

Create `.vscode/launch.json`:
```json
{
    "version": "0.2.0",
    "configurations": [
        {
            "name": "Listen for Xdebug",
            "type": "php",
            "request": "launch",
            "port": 9003,
            "pathMappings": {
                "/var/www/html": "${workspaceFolder}"
            }
        }
    ]
}
```

## Quick Start Summary

For experienced developers, here's the quick setup:

```bash
# 1. Create project and copy all template files
mkdir wp-plugin-dev && cd wp-plugin-dev

# 2. Make wp.sh executable
chmod +x wp.sh

# 3. Create directory structure and .env with random ports
make setup

# 4. Set your user ID for Podman
echo "HOST_UID=$(id -u)" >> .env
echo "HOST_GID=$(id -g)" >> .env

# 5. Build and start
make build
make up
make install

# 6. Start development
make plugin-install
make plugin-dev

# 7. Set up testing (optional)
make install-e2e-deps
```

## Testing Best Practices

### PHPUnit Testing
- Write tests for all plugin functionality
- Use WordPress test factories for creating test data
- Mock external API calls
- Test both success and failure scenarios
- Run tests before committing code

### E2E Testing with Playwright
- Test critical user workflows
- Use page object pattern for maintainability
- Test across different browsers
- Take screenshots on failures
- Run tests in CI/CD pipeline

### Continuous Integration
Example GitHub Actions workflow:

```yaml
name: Plugin Tests
on: [push, pull_request]

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - name: Start containers
        run: |
          cp .env.example .env
          make build
          make up
          make install
      - name: Run PHPUnit tests
        run: make test-php
      - name: Run E2E tests
        run: |
          make install-e2e-deps
          make test-e2e
```

## Common Podman-Specific Issues

### 1. Permission Denied Errors
```bash
# Fix with proper user mapping
podman unshare chown -R $(id -u):$(id -g) ./your-plugin
```

### 2. SELinux Context Issues
```bash
# If :Z labels don't work, try
sudo setenforce 0  # Temporary, for testing only
```

### 3. Rootless Podman Networking
```bash
# If ports don't bind, check:
sysctl net.ipv4.ip_unprivileged_port_start
# Should be 80 or lower
```

### 4. Container Name Collisions
**Problem**: Container names already exist from previous runs

**Solution**: This setup automatically prevents collisions using:
- Random container suffixes (`CONTAINER_SUFFIX` in .env)
- Unique volume names with suffixes
- Network names with suffixes

If you still encounter issues:
```bash
# Clean up old containers/volumes
make clean
# Or manually:
podman rm -f $(podman ps -aq)
podman volume prune -f
```

### 5. Environment Variable Expansion Issues
**Problem**: Podman doesn't expand `${VARIABLE:-default}` in docker-compose.yml

**Solutions**:

1. **Use the .env file approach** (Recommended):
   ```bash
   # Ensure .env file exists and is properly formatted
   make setup
   ```

2. **Manual WordPress installation**:
   ```bash
   # Use the manual install command when environment variables fail
   make install-manual
   ```

3. **Export variables before running commands**:
   ```bash
   export WP_PORT=8080
   export WP_ADMIN_USER=admin
   export WP_ADMIN_PASSWORD=admin123
   podman-compose up
   ```

4. **Direct WP-CLI commands**:
   ```bash
   # If make install fails, install WordPress manually
   make wp ARGS="core install --url=http://localhost:8080 --title='My Site' --admin_user=admin --admin_password=admin123 --admin_email=admin@example.com --skip-email"
   ```

## Conclusion

This setup provides a robust, reproducible WordPress development environment optimized for plugin development with full support for both Docker and Podman. All template files are included above to minimize setup errors.

Key features:
- Complete template files embedded in documentation
- Podman-specific configurations for rootless containers
- **Collision Prevention**: Random container names and volume suffixes
- **Random Port Generation**: Automatic assignment of non-conflicting ports
- Automated permission handling
- Development tools pre-configured
- Email testing with MailHog
- Database management with PHPMyAdmin
- Hot-reload development workflow
- PHPUnit testing integrated in WordPress container
- Playwright E2E testing in dedicated container
- WP-CLI passthrough script for easy command execution

Remember to:
- Always copy template files exactly as shown
- Use `make setup` for automatic .env generation with random ports
- Set proper user IDs in .env for Podman
- Make wp.sh executable with `chmod +x wp.sh`
- Use `make generate-ports` to get new random ports if needed
- Use the Makefile commands for consistency
- Check logs if services don't start properly
- Keep your containers updated
- Write tests for your plugin functionality
- Run tests before deploying

## Updating Your CLAUDE.md File

To make Claude Code aware of your WordPress development environment, add this section to your project's `CLAUDE.md` file:

```markdown
# WordPress Plugin Development Environment

This project uses a containerized WordPress development setup with Podman/Docker compatibility.

## Key Configuration Details

- **Container Runtime**: Supports both Docker and Podman with automatic detection
- **Port Configuration**: Uses random ports to prevent conflicts (check .env file)
- **Container Names**: Uses unique suffixes to prevent naming collisions
- **Apache Port**: Configured to use port 8080 (non-privileged) instead of port 80

## Available Make Commands

- `make setup` - Create project structure and .env with random ports
- `make generate-ports` - Generate new random port numbers
- `make build` - Build container images
- `make up` - Start all services  
- `make install` - Install WordPress automatically
- `make install-manual` - Manual WordPress install (better for Podman)
- `make down` - Stop all services
- `make wp ARGS="command"` - Run WP-CLI commands
- `make plugin-activate` - Activate the development plugin
- `make test` - Run all tests (PHPUnit + Playwright)
- `make clean` - Remove all containers and volumes

## Environment Files

- `.env` - Contains ports, database credentials, and container configuration
- `.env.example` - Template with comments for port generation

## Podman-Specific Notes

- Environment variables may not expand automatically in compose files
- Use `make install-manual` if automatic installation fails
- Container names include random suffixes to prevent collisions
- Apache runs on port 8080 to avoid privilege issues

## Development URLs

Check your .env file for actual port numbers:
- WordPress: http://localhost:${WP_PORT}
- PHPMyAdmin: http://localhost:${PMA_PORT}  
- MailHog: http://localhost:${MAILHOG_PORT}

## Common Troubleshooting

If containers fail to start:
1. Run `make down && make clean` to reset
2. Run `make setup` to regenerate .env with new ports
3. Run `make build && make up`
4. Use `make install-manual` for WordPress setup
```

## Quick CLAUDE.md Update Command

You can quickly add this to your project's CLAUDE.md:

```bash
# Add WordPress development info to CLAUDE.md
cat >> CLAUDE.md << 'EOF'

# WordPress Plugin Development Environment
This project uses a containerized WordPress setup optimized for Podman/Docker.

## Key Commands
- `make setup` - Initialize with random ports
- `make build && make up` - Start environment  
- `make install-manual` - Install WordPress (Podman-friendly)
- `make wp ARGS="plugin list"` - Run WP-CLI commands

## Configuration
- Random ports prevent conflicts (check .env)
- Apache uses port 8080 (non-privileged)
- Container names have unique suffixes
- Environment variables in .env file
EOF
```

Happy plugin development!