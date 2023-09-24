### Setting Up Apache NiFi
We will utilize Apache NiFi to extract data from the bus API and deposit it into the MySQL database, making the data accessible for further consumption. Apache NiFi, a scalable and versatile data engineering tool, will be deployed using Docker on an AWS EC2 instance.

#### Step-by-step Guide
1. **Create a New AWS EC2 Instance**:
   - Choose the instance type and key pair. Opt for t3.xlarge or t3.2xlarge for better performance.
   - You can create a new key pair or use an existing one.
   - Use a security group with an "All Traffic" rule, or create a new security group.
   - Allocate ample storage (e.g., at least 8 GB) as this instance will collect data from the API.
2. **SSH into the EC2 Instance**:
```sh
ssh -i ~/.ssh/your_pem_file.pem ec2-user@'<your_EC2_external_IP>'
```
3. **Install Docker on EC2**:
```sh
sudo yum update -y
sudo yum install docker -y
```
4. **Start Docker Services**:
```sh
sudo service docker start
sudo systemctl enable docker
sudo usermod -a -G docker ec2-user
```
5. **Exit and Reconnect to EC2**:
After configuring Docker, exit and reconnect to the EC2 instance to apply the changes.
```sh
exit
ssh -i ~/.ssh/your_pem_file.pem ec2-user@'<your_EC2_external_IP>'
```

6. **Verify Docker Installation**:
Check that Docker is installed correctly and the service is running.
```sh
docker --version
sudo service docker status
```

### Troubleshooting
If you encounter any issues during the installation, refer to the [Docker Installation Guide](https://docs.docker.com/engine/install/) for troubleshooting tips and detailed installation instructions.

### Additional Configurations
For advanced configurations and usage of Apache NiFi, please refer to the [Official Apache NiFi Documentation](https://nifi.apache.org/docs.html)

### Next Steps
Once Apache NiFi is set up, proceed to [Setting Up CDC (Debezium)](CDCSetup.md) to configure Change Data Capture with Debezium.
