```
services:
  db:
    image: mariadb:10.6
    container_name: nextcloud-db
    restart: always
    command: --transaction-isolation=READ-COMMITTED --log-bin=binlog --binlog-format=ROW
    volumes:
      - db_data:/var/lib/mysql
    environment:
       MYSQL_ROOT_PASSWORD: ${MYSQL_ROOT_PASSWORD}
       MYSQL_PASSWORD: ${MYSQL_PASSWORD}
       MYSQL_DATABASE: ${MYSQL_DATABASE}
       MYSQL_USER: ${MYSQL_USER}
    networks:
      - matter-net
    healthcheck:
      test: ["CMD", "healthcheck.sh", "--connect", "--innodb_initialized"]
      interval: 30s
      timeout: 10s
      retries: 5
      start_period: 60s
    logging:
      driver: json-file
      options:
        max-size: "10m"
        max-file: "3"
  redis:
    image: redis:7.4-alpine
    container_name: nextcloud-redis
    restart: always
    command: >
      redis-server
      --requirepass ${REDIS_HOST_PASSWORD}
      --maxmemory 256mb
      --maxmemory-policy allkeys-lru
      --save 60 1000
      --appendonly yes
      --tcp-backlog 511
    volumes:
      - redis_data:/data
    networks:
      - matter-net
    healthcheck:
      test: ["CMD", "redis-cli", "-a", "${REDIS_HOST_PASSWORD}", "ping"]
      interval: 30s
      timeout: 10s
      retries: 5
    logging:
      driver: json-file
      options:
        max-size: "10m"
        max-file: "3"

  app:
    image: docker.arvancloud.ir/nextcloud:33.0.2
    #    image: nextcloud:stable
    container_name: nextcloud-app
    restart: always
    ports:
      - 8080:80
    volumes:
      - nextcloud_html:/var/www/html
      - nextcloud_data:/var/www/html/data
      - nextcloud_config:/var/www/html/config
      - nextcloud_apps:/var/www/html/custom_apps
      - nextcloud_themes:/var/www/html/themes
    environment:
      # ── Database
      MYSQL_HOST: db:3306
      MYSQL_DATABASE: ${MYSQL_DATABASE}
      MYSQL_USER: ${MYSQL_USER}
      MYSQL_PASSWORD: ${MYSQL_PASSWORD}

      # ── Redis
      REDIS_HOST: redis
      REDIS_HOST_PORT: 6379
      REDIS_HOST_PASSWORD: ${REDIS_HOST_PASSWORD}

      # ── Admin
      NEXTCLOUD_ADMIN_USER: ${NEXTCLOUD_ADMIN_USER}
      NEXTCLOUD_ADMIN_PASSWORD: ${NEXTCLOUD_ADMIN_PASSWORD}
      NEXTCLOUD_TRUSTED_DOMAINS: ${NEXTCLOUD_TRUSTED_DOMAINS}
    depends_on:
      db:
        condition: service_healthy
      redis:
        condition: service_healthy
    networks:
      - matter-net
    logging:
      driver: json-file
      options:
        max-size: "20m"
        max-file: "5"

  collabora:
    image: collabora/code:latest
    container_name: nextcloud-collabora
    restart: always
    ports:
      - 9980:9980
    environment:
      - aliasgroup1=http://localhost:8080,http://nextcloud-app:80
      - username=${COLLABORA_USERNAME}
      - password=${COLLABORA_PASSWORD}
      - extra_params=--o:ssl.enable=false --o:ssl.termination=true
      - dictionaries=en_US,fa_IR
    cap_add:
      - MKNOD
    networks:
      - matter-net
    healthcheck:
      test: ["CMD", "curl", "-f", "http://localhost:9980/"]
      interval: 30s
      timeout: 10s
      retries: 5
      start_period: 60s
    logging:
      driver: json-file
      options:
        max-size: "10m"
        max-file: "3"

networks:
  matter-net:
    external: true
volumes:
  nextcloud_html:
  nextcloud_data:
  nextcloud_config:
  nextcloud_apps:
  nextcloud_themes:
  db_data:
  redis_data:

```
### .env
```
# ────────── Database ──────────
MYSQL_ROOT_PASSWORD=V3ryStr0ng!R00tP@ss_2026
MYSQL_DATABASE=nextcloud
MYSQL_USER=nextcloud
MYSQL_PASSWORD=S3cur3_NC_DB!P@ss_2026

# ────────── Nextcloud ──────────
NEXTCLOUD_ADMIN_USER=admin
NEXTCLOUD_ADMIN_PASSWORD=Ch@ng3Me!Adm1n_2026
NEXTCLOUD_TRUSTED_DOMAINS=172.24.11.168  #cloud.example.com
# ────────── Redis ──────────
REDIS_HOST_PASSWORD=R3d1s!S3cur3_P@ss_2026
# ---------- Collabora -----------------
# Collabora
COLLABORA_USERNAME=admin
COLLABORA_PASSWORD=2wsx@WSX
```
