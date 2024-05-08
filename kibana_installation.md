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

