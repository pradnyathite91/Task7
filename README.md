# Netdata Monitoring on Amazon Linux (Docker)

## Objective
Monitor EC2 instance system metrics using Netdata in Docker.

## Tools
- Amazon Linux EC2
- Docker
- Netdata

# 📊 Monitor Your EC2 Instance with Netdata (Docker-based)

This guide helps you set up **Netdata** monitoring on an EC2 instance using **Docker**. Netdata provides real-time performance metrics such as CPU, memory, disk I/O, network, and Docker containers.

---

## 🧰 Prerequisites

- EC2 instance running Amazon Linux or similar
- Docker installed (covered below)
- SSH access to EC2
- Port `19999` open in EC2 security group

---

## 2️⃣ Update System and Install Docker

```bash
sudo yum update -y
sudo yum install docker -y
```
3️⃣ Start and Enable Docker
To ensure Docker starts on boot and is active now:

```bash
sudo systemctl start docker
sudo systemctl enable docker
You can verify Docker is running using:
```
```bash
sudo systemctl status docker
4️⃣ Run Netdata in Docker
Now run the Netdata container:
```
```bash
sudo docker run -d --name=netdata -p 19999:19999 --cap-add=SYS_PTRACE \
  -v netdataconfig:/etc/netdata \
  -v netdatalib:/var/lib/netdata \
  -v netdatacache:/var/cache/netdata \
  -v /etc/passwd:/host/etc/passwd:ro \
  -v /etc/group:/host/etc/group:ro \
  -v /proc:/host/proc:ro \
  -v /sys:/host/sys:ro \
  -v /etc/os-release:/host/etc/os-release:ro \
  netdata/netdata
```
This pulls and starts Netdata as a background container, accessible on port 19999.

5️⃣ Allow Port 19999 in Security Group
To access the Netdata dashboard from a browser:

Open the EC2 Console

Go to Security Groups

Choose the group attached to your instance

Click Inbound rules > Edit inbound rules

Add the following rule:
Type: Custom TCP
Port Range: 19999
Source: 0.0.0.0/0   # OR use your specific IP for more security
Click Save rules.

6️⃣ Access Netdata Dashboard
Once Docker is running and port 19999 is open, access Netdata by visiting:

text
Copy
Edit
http://<your-ec2-public-ip>:19999
Replace <your-ec2-public-ip> with your EC2 instance's public IP address.

7️⃣ Explore the Dashboard
With Netdata running, you can monitor:

🧠 CPU Usage

📈 Memory and RAM

💾 Disk I/O

🔧 System performance metrics

🐳 Docker container statistics

🚨 Alerts and system health checks

📊 Real-time visual charts

8️⃣ Check Netdata Logs (Optional)
To inspect Netdata’s internal logs:

bash
Copy
Edit
docker exec -it netdata bash
cat /var/log/netdata/error.log

