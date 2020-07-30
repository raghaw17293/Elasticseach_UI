# Elasticseach_UI

# ELK setup - 
Elasticsearch installation for each node :- 
## Download binaries with command - 
     wget https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-7.7.0-linux-x86_64.tar.gz
     tar -xzf elasticsearch-7.7.0-linux-x86_64.tar.gz 

# Java OpenJDK installation :
## Check wether java is install properly by typing : 
    java -version
If java is installed it will give - openjdk version "version_no.".
## If not then install it by : 
    sudo apt install openjdk-11-jdk

# Kibana binaries installation :
    wget https://artifacts.elastic.co/downloads/kibana/kibana-7.2.0-linux-x86_64.tar.gz
    tar -xzf kibana-7.2.0-linux-x86_64.tar.gz

## Making changes to Elasticsearch configuration files. 
    • Go to elasticsearch directory (where it was extracted) → config → elasticsearch.yml
Everything is commented out initially. Things to specify
    • <cluster.name: my_cluster_name>
    • <node.name: node_name>
    • For Master node: <node.master: true>
    • For Data nodes: <node.data: true>
    • <network.host: host_ip_address>
    • <http.port: 9200>

# To start elasticsearch,
    • Enter <su username> and enter password,
    • <cd elasticsearch_directory>  // Deirectory Where you have extracted elasticsearch.gz file
    • Type : bin/elasticsearch // It will start elastic search cluster



# Setting up Kibana

## Make changes in “kibana.yml” file in config directory of Kibana.
    • <cd kibana_directory/config>
    • Open “kibana.yml” file in it. Enter the following :
    • <server.port: 5601>
    • <server.host: host_ip_address> i.e. IP of node that contains Kibana
    • <elasticsearch.hosts: ["http://host_ip_address:9200"]>

# To start kibana:
    • <cd kibana_directory>
    • <bin/kibana>

# Setting up Kibana Index Pattern

Let’s say the name of the index (data) that we pushed through Spark is coriolis_data.
And this data contains many fields including a field named, say “image” which contains base64 string of an image.
## To view data, we need to create an “Index Pattern”. Following are the steps :
    • On left most part of Kibana page, there is a panel of different tools. Select “Management” tab from it which is at the bottom.
    • Inside it, go to “Index patterns” tab and click on “Create new Index pattern”.
    • Then it prompts to provide a name for “Index pattern” which must be either the same as the name of “index” (data) or at least a prefix of the index name.
    • Once the index pattern is created, you can see different fields and their types.
    • Go to “image” field, click the edit icon at the rightmost end and make the following changes:
    • Format → <Url>
    • Type → <Image>
    • URL template → <data:image/jpeg;base64,{{rawValue}}>
    • and click on “save field”.

Kibana is ready now with an Index pattern which allows to do all searches, visualizations, etc.

# appbaseio/dejavu setup :-
## Docker Installation :
     docker run -p 1358:1358 -d appbaseio/dejavu:3.3.0
Then Go to browser and open :- http://localhost:1358/
## To run dejavu locally :-
     sudo docker run appbaseio/dejavu
# Cross-origin resource sharing (CORS) :
1. Open path_to_elasticsearch/config/elasticsearch.yml file.
2. Add the following lines to allow dejavu and reactivesearch:
## 
       http.port: 9200
       http.cors.allow-origin:http://ip_address:1358;,http://ip_address:3000,http://ip_address:3001,https://opensource.appbase.io/dejavu/live,https://dejavu.appbase.io
       http.cors.enabled: true
       http.cors.allow-headers: X-Requested-With,X-Auth-Token,Content-Type,Content-Length,Authorization
       http.cors.allow-credentials: true
Go to http://localhost:1358/
Then type http://localhost:9200/ in “header” bar and type elasticsearch index
in “appbase(aka index)” .Click on connect button.

# Reactivesearch Setup :-
## Installing npm :-
    sudo apt install npm
Skip step first if already installed.

## Installing reactivesearch -
    npm install @appbseio/reactivesearch
    npm install -g create-react-app
    create-react-app name_of_your_app
# for tag cloud
open terminal and cd to name_of _your_app
  ## 
     npm i tag-cloud 
     cd name_of_your_app
     npm start
 Ope browser and type: localhost:3000
  
# Steps to show images or Videos :-
1. Go to name_of_your_app/public directory and copy all images here .
(for instance : in mine case - i have path as one field where path for all images is defined as
&#39;path&#39;: &#39;frames1/name_of_frame.PNG; .So i created folder named frames1 inside
name_of_your_app/public folder and copied all my images/frames corresponding to camera_id
1 to it .Similarly, for camera_id 2 .
And for Viedios I have folder named “Videos” inside name_of_your_app/public and all mine
videos are inside this folder .)
2. Go to name_of_your_app/src folder and under open App.js and add your components here .
(in our case just copy and paste the code in it and just change &lt;ReactiveBase
app=”elasticsearch_index_name&quot;
url =&quot;http://localhost:9200&quot;
&gt;
.)
3. relod the page it must render all images and Videos or images .(in our case it render only if
data pushed to es cluster has the same mapping that i defined in python script.)
#index.max_inner_result_window in es to change size.

