# qbittorrent-port-forward-gluetun-server

A shell script and Docker container for automatically setting qBittorrent's listening port from Gluetun's control server.

## Config

### Environment Variables

| Variable     | Example                                     | Default                      | Description                                                     |
|--------------|---------------------------------------------|------------------------------|-----------------------------------------------------------------|
| QBT_USERNAME | `username`                                  | `admin`                      | qBittorrent username                                            |
| QBT_PASSWORD | `password`                                  | `adminadmin`                 | qBittorrent password                                            |
| QBT_ADDR     | `http://192.168.1.100:8080`                 | `http://localhost:8080`      | HTTP URL for the qBittorrent web UI, with port                  |
| GTN_ADDR     | `http://192.168.1.100:8000`                 | `http://localhost:8000`      | HTTP URL for the gluetun control server, with port              |
| API_KEY      | See [authentication](#authentication)       | `-`                          | API key for authentication with the gluetun server              |

## Example

### Docker-Compose

The following is an example docker-compose:

```yaml
  qbittorrent-port-forward-gluetun-server:
    image: ghcr.io/deliciousbroccoli/qbittorrent-port-forward-gluetun-server:latest
    container_name: qbittorrent-port-forward-gluetun-server
    restart: unless-stopped
    environment:
      - QBT_USERNAME=username
      - QBT_PASSWORD=password
      - QBT_ADDR=http://192.168.1.100:8080
      - GTN_ADDR=http://192.168.1.100:8000
      - API_KEY=changeme
```

### Authentication

Check out [Gluetun documentation](https://github.com/qdm12/gluetun-wiki/blob/main/setup/advanced/control-server.md#authentication) and create a config file for API authentication.

Generate API key with `docker run --rm qmcgaw/gluetun genkey`

## Development

### Build Image

`docker build . -t qbittorrent-port-forward-gluetun-server`

### Run Container

`docker run --rm -it -e QBT_USERNAME=admin -e QBT_PASSWORD=adminadmin -e QBT_ADDR=http://192.168.1.100:8080 -e GTN_ADDR=http://192.168.1.100:8000 -e API_KEY=changeme qbittorrent-port-forward-gluetun-server:latest`
