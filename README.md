# Netdata Monitoring on Amazon Linux (Docker)

## Objective
Monitor EC2 instance system metrics using Netdata in Docker.

## Tools
- Amazon Linux EC2
- Docker
- Netdata

## Run Netdata
```bash
docker run -d --name=netdata -p 19999:19999 netdata/netdata

Access
http://<EC2-IP>:19999

Monitored Metrics
CPU, Memory, Disk, Docker containe
