# Kibana quick start: installation

Download and install the Linux 64-bit package
The Linux archive for Kibana v8.13.3 can be downloaded and installed as follows:
  ```bash
  curl -O https://artifacts.elastic.co/downloads/kibana/kibana-8.13.3-linux-x86_64.tar.gz
  curl https://artifacts.elastic.co/downloads/kibana/kibana-8.13.3-linux-x86_64.tar.gz.sha512 | shasum -a 512 -c - 
  tar -xzf kibana-8.13.3-linux-x86_64.tar.gz
  cd kibana-8.13.3/ 
  ```

## Start Elasticsearch and generate an enrollment token for Kibana

When you start Elasticsearch for the first time, the following security configuration occurs automatically:
- Certificates and keys for TLS are generated for the transport and HTTP layers.
- The TLS configuration settings are written to elasticsearch.yml.
- A password is generated for the elastic user.
- An enrollment token is generated for Kibana.

You can then start Kibana and enter the enrollment token to securely connect Kibana with Elasticsearch. The enrollment token is valid for specified time.

If Kibana service running on port 5601 is not accessible from a different server, it indicates a networking issue. Here are some steps to troubleshoot and resolve the problem:
1. Firewall Rules: Ensure that the firewall on the server running Kibana allows incoming connections on port 5601. You can check and update the firewall rules based on your operating system. For example, using iptables on Linux:
  ```bash
  sudo iptables -I INPUT -p tcp --dport 5601 -j ACCEPT
  ```
Or using firewalld:
  ```bash
  sudo firewall-cmd --zone=public --add-port=5601/tcp --permanent
  sudo firewall-cmd --reload
  ```
2. Check if Kibana is configured to bind to the correct network interface or IP address. By default, Kibana binds to localhost only. You may need to update the server.host setting in the kibana.yml configuration file to bind to 0.0.0.0 or the specific IP address of the server:
  ```bash
  server.host: "0.0.0.0"
  ```

