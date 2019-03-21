# Prometheus for Windows

This is a repository with Prometheus pull servers in Windows Docker containers.

## Usage

Run the following command to get this container running! There is also support for previous Windows versions.

```bash
# Windows Server 1809
docker run --rm -p 9090:9090 johnnyhuy/prometheus-windows:latest

# OR

# Windows Server 1803
docker run --rm -p 9090:9090 johnnyhuy/prometheus-windows:1803

# OR

# Windows Server 2016
docker run --rm -p 9090:9090 johnnyhuy/prometheus-windows:win2016
```

## Prerequisites

- Docker & Windows (duhh!)

## Contribution

- Project derived from [StefanScherer/dockerfiles-windows](https://github.com/dockersamples/aspnet-monitoring)
- **Johnny Huynh** - Initial changes in this repository

## License

This project is licensed under the MIT license, see [LICENSE](https://github.com/johnnyhuy/grafana-windows/blob/master/LICENSE) for more information.

- **StefanScherer/dockerfiles-windows** is licensed under the [MIT License](https://github.com/StefanScherer/dockerfiles-windows/blob/master/LICENSE)
