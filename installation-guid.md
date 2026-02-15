1. System Preparation
The system was updated to ensure latest security patches and dependency compatibility before Elasticsearch installation.

sudo apt update & sudo apt upgrade

2. Required Dependencies
Installed required packages to enable secure repository access and package verification.
sudo apt install curl apt-transport-https gnupg -y

curl
What It Is:

A command-line tool used to transfer data from or to a server using protocols like HTTP, HTTPS, FTP, etc.

Why We Need It:

We use curl to download Elastic’s official GPG key securely from their website.