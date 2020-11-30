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
add the master node's public ip for internet access
installed elasticsearch - gpg key installed

- Install Kibana on the master-1 node.
- $ sudo su -
- $ curl -O https://artifacts.elastic.co/downloads/kibana/kibana-7.6.0-x86_64.rpm
- Install Kibana:
- $ rpm --install kibana-7.6.0-x86_64.rpm
- Configure Kibana to start on system boot:
- $ systemctl enable kibana

- configure kibana
- log in to master-1 as root
- $ sudo su - 
- $ vim /etc/kinana/kibana.yml
  - change the following line:
  - #server.port: 5601 to server.port: 8080
  - change the following line: #server.host: "localhost" to server.host: "10.0.1.101" <ip of masternode1> 

- start kibana 

- $ sudo  su -
- $ systemctl start kibana 
- $ tail -f /var/log/message

- Use Kibana's Console tool:
- after kibana has finished starting up.  Navigate to http://public_Ip_ADDRESS_OF_MASTER-1:8080
- Check the node status of the cluster via the console tool with:
- $ GET _cat/nodes?


# Deploy and Configure a Multi-Node Elasticsearch Cluster
- 6 nodes: 3 masters and 3 datas
- configure each node to listen to localhost and their site local address(talk-2each-other)
- Note: arechive installation of elasticsearch therefore manually set the oopen file limit for the elastic user and virtual memory map max count

- start master1 task 1:
    - create the elastic user
      - $ sudo user add elastic
    - Open the limits.conf as root elasticsearch checks this, has to be 65536 files open 2 work
      - $ sudo vim /etc/security/limits.conf
    - Add the following line neaer th bottom:
      - elastic - nofile 65536
    -  open the sysctl.conf file as root:
      - sudo vim /etc/sysctl.conf
    -  add the following line at the bottom:
      - vm.max_map_count=262144
    - load the new sysctl values:
      - sudo sysctl -p
    - Become the elastic user:
      - sudo su - elastic
    -  Download the binaries for Elasticsearch 7.2.1 in the elastic user's home directory:
      - curl -O https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-7.2.1-linux-x86_64.tar.gz
    - Unpack the archive and make pretty
      - $ tar -xzvf elasticsearch-7.2.1-linux-x86_64.tar.gz
      - $ rm elasticsearch-7.2.1-linux-x86_64.tar.gz
      - $ mv elasticsearch-7.2.1 elasticsearch

# Configure each node's elasticsearch.yml per instructions.

Log in to each node and become the elastic user:

sudo su - elastic
Open the elasticsearch.yml file:

vim /home/elastic/elasticsearch/config/elasticsearch.yml
Change the following line:

#cluster.name: my-application
to

cluster.name: roroxas_cluster
Change the following line on master-1:

#node.name: node-1
to

node.name: master-1
Change the following line on master-2:

#node.name: node-1
to

node.name: master-2
Change the following line on master-3:

#node.name: node-1
to

node.name: master-3
Change the following line on data-1:

#node.name: node-1
to

node.name: data-1
Change the following line on data-2:

#node.name: node-1
to

node.name: data-2
Change the following line on data-3:

#node.name: node-1
to

node.name: data-3
Change the following line on master-1:

#node.attr.rack: r1
to

node.attr.zone: 1
Change the following line on master-2:

#node.attr.rack: r1
to

node.attr.zone: 2
Change the following line on master-3:

#node.attr.rack: r1
to

node.attr.zone: 3
Change the following line on data-1:

#node.attr.rack: r1
to

node.attr.zone: 1
Add the following line on data-1:

node.attr.temp: hot
Change the following line on data-2:

#node.attr.rack: r1
to

node.attr.zone: 2
Add the following line on data-2:

node.attr.temp: hot
Change the following line on data-3:

#node.attr.rack: r1
to

node.attr.zone: 3
Add the following line on data-3:

node.attr.temp: warm
Add the following lines on master-1:

node.master: true
node.data: false
node.ingest: false
Add the following lines on master-2:

node.master: true
node.data: false
node.ingest: false
Add the following lines on master-3:

node.master: true
node.data: false
node.ingest: false
Add the following lines on data-1:

node.master: false
node.data: true
node.ingest: true
Add the following lines on data-2:

node.master: false
node.data: true
node.ingest: true
Add the following lines on data-3:

node.master: false
node.data: true
node.ingest: false
Change the following on each node:

#network.host: 192.168.0.1
to

network.host: [_local_, _site_]
Change the following on each node:

#discovery.seed_hosts: ["host1", "host2"]
to

discovery.seed_hosts: ["10.0.1.101", "10.0.1.102", "10.0.1.103"]
Change the following on each node:

#cluster.initial_master_nodes: ["node-1", "node-2"]
to

cluster.initial_master_nodes: ["master-1", "master-2", "master-3"]
# Configure the heap for each node per instructions.

Log in to each master node and become the elastic user:

sudo su - elastic
Open the jvm.options file:

vim /home/elastic/elasticsearch/config/jvm.options
Change the following lines:

-Xms1g
-Xmx1g
to

-Xms768m
-Xmx768m
Log in to each data node and become the elastic user:

sudo su - elastic
Open the jvm.options file:

vim /home/elastic/elasticsearch/config/jvm.options
Change the following lines:

-Xms1g
-Xmx1g
to

-Xms2g
-Xmx2g1

# Start Elasticsearch as a daemon on each node.
Log in to each node and become the elastic user:

sudo su - elastic
Switch to the elasticsearch directory:

cd /home/elastic/elasticsearch
Start Elasticsearch as a daemon:

./bin/elasticsearch -d -p pid
Check the startup process:

less /home/elastic/elasticsearch/logs/linux_academy.log
Check the node configuration:

curl localhost:9200/_cat/nodes?v
