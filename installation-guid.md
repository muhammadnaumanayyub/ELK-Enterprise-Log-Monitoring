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