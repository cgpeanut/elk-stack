The ELK Stack

What is the ELK stack?
The ELK stack is an acronym used to describe a stack that comprises of three popular open-source projects: Elasticsearch, Logstash, and Kibana. Often referred to as Elasticsearch, the ELK stack gives you the ability to aggregate logs from all your systems and applications, analyze these logs, and create visualizations for application and infrastructure monitoring, faster troubleshooting, security analytics, and more.

E = Elasticsearch
Elasticsearch is an open-source, RESTful, distributed search and analytics engine built on Apache Lucene. Support for various languages, high performance, and schema-free JSON documents makes Elasticsearch an ideal choice for various log analytics and search use cases.

L = Logstash
Logstash is an open-source data ingestion tool that allows you to collect data from a variety of sources, transform it, and send it to your desired destination. With pre-built filters and support for over 200 plugins, Logstash allows users to easily ingest data regardless of the data source or type. Learn more »

K = Kibana
Kibana is an open-source data visualization and exploration tool for reviewing logs and events. Kibana offers easy-to-use, interactive charts, pre-built aggregations and filters, and geospatial support and making it the preferred choice for visualizing data stored in Elasticsearch.

# Deploy and Configure Kibana for an Elasticsearch Cluster 

kibana default port: 5601
default port 8080 

installed elasticsearch - gpg key installed

# Install Kibana on the master-1 node.
- $ sudo su -
- $ curl -O https://artifacts.elastic.co/downloads/kibana/kibana-7.6.0-x86_64.rpm
- Install Kibana:
- $ rpm --install kibana-7.6.0-x86_64.rpm
- Configure Kibana to start on system boot:

# configure kibana
- log in to master-1 as root
- $ sudo su - 
- $ vim /etc/kinana/kibana.yml
  - change the following line:
  - #server.port: 5601 to server.port: 8080
  - change the following line: #server.host: "localhost" to server.host: "10.0.1.101" <ip of masternode1> 

# start kibana 

- $ sudo  su -
- $ systemctl start kibana 

- Use Kibana's Console tool:
    - after kibana has finished starting up.  Navigate to http://public_Ip_ADDRESS_OF_MASTER-1:8080
- Check the node status of the cluster via the console tool with:
    - GET _cat/nodes?v