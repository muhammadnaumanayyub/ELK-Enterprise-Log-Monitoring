1. System Preparation
The system was updated to ensure latest security patches and dependency compatibility before Elasticsearch installation.

sudo apt update & sudo apt upgrade

2. Required Dependencies
Installed required packages to enable secure repository access and package verification.
sudo apt install curl apt-transport-https gnupg -y

--curl
Installed curl to securely retrieve Elastic’s official GPG signing key required for repository authentication.

--apt-transport-https
Installed apt-transport-https to allow secure package installation from Elastic’s HTTPS repository.

--gnupg
Installed gnupg to manage cryptographic keys and verify the authenticity of Elastic’s signed packages

3. Add Elastic GPG Key
To ensure secure package installation, Elastic’s official GPG signing key was added to the system.

curl -fsSL https://artifacts.elastic.co/GPG-KEY-elasticsearch | \
sudo gpg --dearmor -o /usr/share/keyrings/elastic.gpg

The GPG key enables Ubuntu’s APT package manager to verify the authenticity and integrity of Elasticsearch packages before installation. This prevents unauthorized or tampered packages from being installed and ensures that only officially signed Elastic software is trusted.
The key was stored in /usr/share/keyrings/elastic.gpg and later referenced during repository configuration for secure package validation.

4. Add Elastic Repository
echo "deb [signed-by=/usr/share/keyrings/elastic.gpg] https://artifacts.elastic.co/packages/8.x/apt stable main" | \
sudo tee /etc/apt/sources.list.d/elastic-8.x.list

Configured official Elastic repository to allow installation and future updates directly from trusted source.

5. Install Elasticsearch
sudo apt install elasticsearch -y

Installed Elasticsearch 8.x as the centralized log indexing and search engine component of the ELK stack.

6. JVM Heap Memory Optimization
sudo nano /etc/elasticsearch/jvm.options
Change 
-Xms1g
-Xmx1g

To 
-Xms512m
-Xmx512m

Elasticsearch runs on the Java Virtual Machine (JVM) and requires heap memory for indexing and search operations.
By default, it allocates 1GB of RAM (-Xms1g and -Xmx1g). Since the lab environment uses 4GB total RAM, the heap size was reduced to:
-Xms512m
-Xmx512m
This optimization prevents memory exhaustion and ensures stable performance in a low-resource environment. Both values were kept equal to maintain JVM stability.

7. Configure Single Node Mode
sudo nano /etc/elasticsearch/elasticsearch.yml
By default, Elasticsearch is designed to operate in cluster mode and attempts to discover other nodes during startup. Since this deployment is a standalone lab environment, the following configuration was added:

discovery.type: single-node
Add above line at the end of file.
This ensures Elasticsearch runs as a single-node instance without attempting cluster formation, preventing discovery-related startup errors.
Also comment the following line
cluster.initial_master_nodes: ["Wazuh"]

8. Enable and Start Service
sudo systemctl daemon-reload
sudo systemctl enable elasticsearch
sudo systemctl start elasticsearch

9. Verify Service
sudo systemctl status elasticsearch
Then open the application is browser 
https://localhost:9200
