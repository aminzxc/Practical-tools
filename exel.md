```
services:
  grist:
    image: gristlabs/grist:latest
    container_name: exel
    restart: unless-stopped
    environment:
      GRIST_SANDBOX_FLAVOR: gvisor
      GRIST_SINGLE_ORG: "true"
      GRIST_DEFAULT_EMAIL: amin16302@gmail.com
      GRIST_DOMAIN: localhost
      GRIST_FORCE_LOGIN: "false"
    volumes:
      - ./data:/persist
    ports:
      - "8484:8484"
    networks:
      - matter-net
networks:
  matter-net:
    external: true
volumes:
  data:
```
