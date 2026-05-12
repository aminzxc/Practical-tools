### draw.io (diagrams.net) is a web-based diagramming application used to create visual diagrams such as flowcharts, system architecture diagrams, UML diagrams, network diagrams, and mind maps
```
services:
  drawio:
    image: docker.io/jgraph/drawio
    container_name: diagram
    restart: unless-stopped
    ports:
      - 8090:8080
    healthcheck:
      test: ["CMD-SHELL", "curl -f http://localhost:8080 || exit 1"]
      interval: 1m30s
      timeout: 10s
      retries: 5
      start_period: 10s
    networks:
      - matter-net
networks:
  matter-net:
    external: true

```
