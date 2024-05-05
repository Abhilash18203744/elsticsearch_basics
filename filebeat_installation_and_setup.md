# Filebeat quick start: installation and configuration
  This guide describes how to get started quickly with log collection.
  - install Filebeat on each system you want to monitor
  - specify the location of your log files
  - parse log data into fields and send it to Elasticsearch
  - visualize the log data in Kibana

## Step 1: Install Filebeat
  Install Filebeat on all the servers you want to monitor.
  To download and install Filebeat, use the commands that work with your system: (for linux)
  ```bash
  curl -L -O https://artifacts.elastic.co/downloads/beats/filebeat/filebeat-8.13.3-linux-x86_64.tar.gz
  tar xzvf filebeat-8.13.3-linux-x86_64.tar.gz
  ```

  For installation using APT: 
  ### Download and install the public signing key:
  ```bash
  wget -qO - https://artifacts.elastic.co/GPG-KEY-elasticsearch | sudo apt-key add -
  ```
  
  ### Install the apt-transport-https packages
  ```bash
  sudo apt-get install apt-transport-https
  ```
  
  ### Save the repository definition to /etc/apt/sources.list.d/elastic-8.x.list:
  ```bash
  echo "deb https://artifacts.elastic.co/packages/8.x/apt stable main" | sudo tee -a /etc/apt/sources.list.d/elastic-8.x.list
  ```
  
  ### Run apt-get update, and the repository is ready for use. For example, you can install Filebeat by running:
  ```bash
  sudo apt-get update && sudo apt-get install filebeat
  ```

  ### To configure Filebeat to start automatically during boot, run:
  ```bash
  sudo systemctl enable filebeat
  ```
  If your system does not use systemd then run:
  ```bash
  sudo update-rc.d filebeat defaults 95 10
  ```

## Step 2: Connect to the Elastic Stack
  Connections to Elasticsearch and Kibana are required to set up Filebeat.
  Set the connection information in filebeat.yml.

  ### Set the host and port where Filebeat can find the Elasticsearch installation, and set the username and password of a user who is authorized to set up Filebeat. For example:
  ```bash
  output.elasticsearch:
  hosts: ["https://myEShost:9200"]
  username: "filebeat_internal"
  password: "YOUR_PASSWORD" 
  ssl:
    enabled: true
    ca_trusted_fingerprint: "b9a10bbe64ee9826abeda6546fc988c8bf798b41957c33d05db736716513dc9c" 
  ```
  Note: The fingerprint is printed on Elasticsearch start up logs, or you can retrieve it from crt file as follows. (refer_link_for_crt)[https://www.elastic.co/guide/en/elasticsearch/reference/8.0/configuring-stack-security.html#_connect_clients_to_elasticsearch_5]
  When you start Elasticsearch for the first time, TLS is configured automatically for the HTTP layer. A CA certificate is generated and stored on disk at:
  ```bash
  /etc/elasticsearch/certs/http_ca.crt
  ```
  Copy the fingerprint value that’s output to your terminal when Elasticsearch starts, and configure your client to use this fingerprint to establish trust when it connects to Elasticsearch.

  If the auto-configuration process already completed, you can still obtain the fingerprint of the security certificate by running the following command. The path is to the auto-generated CA certificate for the HTTP layer.
  ```bash
  openssl x509 -fingerprint -sha256 -in config/certs/http_ca.crt
  ```
  The command returns the security certificate, including the fingerprint. The issuer should be Elasticsearch security auto-configuration HTTP CA.

  Note: If your library doesn’t support a method of validating the fingerprint, the auto-generated CA certificate is created on each Elasticsearch node. Copy the http_ca.crt file to your machine and configure your client to use this certificate to establish trust when it connects to Elasticsearch.
