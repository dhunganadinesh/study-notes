Install Dependencies

Node.js 18+
curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash -
sudo apt install nodejs -y
node --version  # should show v18+

Python & pip
sudo apt install python3 python3-pip -y

Redis
sudo apt install redis-server -y
sudo systemctl enable redis-server
sudo systemctl start redis-server - Done

RabbitMQ
sudo apt install rabbitmq-server -y
sudo systemctl enable rabbitmq-server
sudo systemctl start rabbitmq-server

Enalbe Management Plugin
sudo rabbitmq-plugins enable rabbitmq_management

Install OpenJDK
sudo apt install openjdk-17-jdk -y

Create and Edit
sudo nano /etc/elasticsearch/jvm.options.d/memory.options

Add below
========
-Xms512m
-Xmx512m
=======


Install Elastic Search
wget -qO - https://artifacts.elastic.co/GPG-KEY-elasticsearch | sudo gpg --dearmor -o /usr/share/keyrings/elasticsearch-keyring.gpg

Add the repo
echo "deb [signed-by=/usr/share/keyrings/elasticsearch-keyring.gpg] https://artifacts.elastic.co/packages/8.x/apt stable main" | sudo tee /etc/apt/sources.list.d/elastic-8.x.list

INstall
sudo apt update && sudo apt install elasticsearch -y

Configure Elastic
sudo nano /etc/elasticsearch/elasticsearch.yml

Add these lines
network.host: 127.0.0.1
http.port: 9200
xpack.security.enabled: false

Start it
sudo systemctl enable elasticsearch
sudo systemctl start elasticsearch

Test if it's working
curl http://localhost:9200

Install MiniIO
wget https://dl.min.io/server/minio/release/linux-arm64/minio
chmod +x minio
sudo mv minio /usr/local/bin/

Create data directory
sudo mkdir -p /data/minio

Create MiniIO service
sudo nano /etc/systemd/system/minio.service

Paste this
==============

[Unit]
Description=MinIO
After=network.target

[Service]
User=root
Environment="MINIO_ROOT_USER=opencti"
Environment="MINIO_ROOT_PASSWORD=YourMinioPassword123"
ExecStart=/usr/local/bin/minio server /data/minio --console-address ":9001"
Restart=always

[Install]
[Unit]
Description=MinIO
After=network.target

[Service]
User=root
Environment="MINIO_ROOT_USER=opencti"
Environment="MINIO_ROOT_PASSWORD=YourMinioPassword123"
ExecStart=/usr/local/bin/minio server /data/minio --console-address ":9001"
Restart=always

[Install]
WantedBy=multi-user.target

===================

Start MiniIO
sudo systemctl daemon-reload
sudo systemctl enable minio
sudo systemctl start minio

Download OpenCTI
cd /opt
sudo git clone https://github.com/OpenCTI-Platform/opencti.git
cd opencti/opencti-platform/opencti-graphql


Install Node Dependencies
sudo npm install


Configure OpenCTI
sudo nano config/production.json

Set it up like this
{
  "app": {
    "port": 8080,
    "base_path": ""
  },
  "admin": {
    "email": "admin@opencti.io",
    "password": "YourStrongPassword123",
    "token": "paste-a-uuid-here"
  },
  "redis": {
    "hostname": "127.0.0.1",
    "port": 6379
  },
  "elasticsearch": {
    "url": "http://127.0.0.1:9200"
  },
  "minio": {
    "endpoint": "127.0.0.1",
    "port": 9000,
    "use_ssl": false,
    "access_key": "opencti",
    "secret_key": "YourMinioPassword123"
  },
  "rabbitmq": {
    "hostname": "127.0.0.1",
    "port": 5672,
    "username": "guest",
    "password": "guest"
  }
}

Generate UUID for the token
cat /proc/sys/kernel/random/uuid

Build and start OpenCTI
sudo npm run build

Start OpenCTI
sudo node dist/boot.js &

Make OpenCTI system service
sudo nano /etc/systemd/system/opencti.service

======
[Unit]
Description=OpenCTI
After=network.target elasticsearch.service redis.service rabbitmq-server.service minio.service

[Service]
WorkingDirectory=/opt/opencti/opencti-platform/opencti-graphql
ExecStart=/usr/bin/node dist/boot.js
Restart=always
Environment=NODE_ENV=production

[Install]
WantedBy=multi-user.target
============
sudo systemctl daemon-reload
sudo systemctl enable opencti
sudo systemctl start opencti


Quick Troubleshoot
IssueFixElasticsearch not startingCheck sudo journalctl -u elasticsearchPort 9200 not respondingCheck xpack.security.enabled: false is setnpm install failsMake sure Node 18+ is installedMinIO ARM64 errorMake sure you used the linux-arm64 MinIO binaryCan't reach localhost:8080Check sudo systemctl status opencti