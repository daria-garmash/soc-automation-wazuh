<h3>Step 1. You’ll need at least 4 machines (see Technical Resources)</h3>
<h3>Step 2. Set up Wazuh Manager (on Ubuntu)</h3>
<ol>
  <li>Download Wazuh</li>
    <code>curl -sO https://packages.wazuh.com/4.7/wazuh-install.sh && sudo bash ./wazuh-install.sh -a </code>
  <li>Extract credentials for Wazuh in case you forget them </li>
    <code>sudo tar -xvf wazuh-install-files.tar</code>
  <li>Check status of wazuh-manager service</li>
    <code>sudo systemctl status wazuh-manager.service </code>
  <li>Access Wazuh Dashboard on https://WAZUH_SERVER_IP:443</li>
</ol>
<h3>Step 3. Set up Wazuh Agent</h3>
<ol>
   <li>Go to Wazuh Dashboard -> Agents</li>
   <li>Add new agent</li>
  <li>Fill in information on Agent’s OS (Ubuntu in this case), Wazuh Server’s IP, and optionally agent’s name.</li>
  <li>Copy the generated string and execute it on the machine that you need to monitor.</li>
  <li>Make sure that the agent appears on Wazuh’s Dashboard.</li>
</ol>
<h3>Step 4. Set up TheHive </h3>
<ol>
  <li>Install dependencies</li>
  <code>apt install wget gnupg apt-transport-https git ca-certificates ca-certificates-java curl  software-properties-common python3-pip lsb-release</code>
  
  <li>Install Java</li>
  <code>wget -qO- https://apt.corretto.aws/corretto.key | sudo gpg --dearmor  -o /usr/share/keyrings/corretto.gpg</code>
  <code>echo "deb [signed-by=/usr/share/keyrings/corretto.gpg] https://apt.corretto.aws stable main" |  sudo tee -a /etc/apt/sources.list.d/corretto.sources.list</code>
  <code>sudo apt update</code>
  <code>sudo apt install java-common java-11-amazon-corretto-jdk</code>
  <code>echo JAVA_HOME="/usr/lib/jvm/java-11-amazon-corretto" | sudo tee -a /etc/environment </code>
  <code>export JAVA_HOME="/usr/lib/jvm/java-11-amazon-corretto"</code>
  
  <li>Install Cassandra </li>
  <code>wget -qO -  https://downloads.apache.org/cassandra/KEYS | sudo gpg --dearmor  -o /usr/share/keyrings/cassandra-archive.gpg</code>
  <code>echo "deb [signed-by=/usr/share/keyrings/cassandra-archive.gpg] https://debian.cassandra.apache.org 40x main" |  sudo tee -a /etc/apt/sources.list.d/cassandra.sources.list</code>
  <code>sudo apt update</code>
  <code>sudo apt install cassandra</code>
  
  <li>Install ElasticSearch</li>
  <code>wget -qO - https://artifacts.elastic.co/GPG-KEY-elasticsearch |  sudo gpg --dearmor -o /usr/share/keyrings/elasticsearch-keyring.gpg</code>
  <code>sudo apt-get install apt-transport-https</code>
  <code>echo "deb [signed-by=/usr/share/keyrings/elasticsearch-keyring.gpg] https://artifacts.elastic.co/packages/7.x/apt stable main" |  sudo tee /etc/apt/sources.list.d/elastic-7.x.list </code>
  <code>sudo apt update</code>
  <code>sudo apt install elasticsearch</code>
  
  <li>Install TheHive </li>
  <code>wget -O- https://archives.strangebee.com/keys/strangebee.gpg | sudo gpg --dearmor -o /usr/share/keyrings/strangebee-archive-keyring.gpg</code>
  <code>echo 'deb [signed-by=/usr/share/keyrings/strangebee-archive-keyring.gpg] https://deb.strangebee.com thehive-5.2 main' | sudo tee -a /etc/apt/sources.list.d/strangebee.list</code>
  <code>sudo apt-get update</code>
  <code>sudo apt-get install -y thehive</code>
  
  <li>Check that TheHive web interface is accessible: http://<THEHIVE_IP>:9000</li>
    <li>Log in to TheHive </li>
    <p>Default login: admin@thehive.local </p>
    <p>Default password: secret</p>
</ol>











