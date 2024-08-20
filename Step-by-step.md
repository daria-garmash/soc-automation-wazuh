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
  <p><code>wget -qO- https://apt.corretto.aws/corretto.key | sudo gpg --dearmor  -o /usr/share/keyrings/corretto.gpg</code></p>
  <p><code>echo "deb [signed-by=/usr/share/keyrings/corretto.gpg] https://apt.corretto.aws stable main" |  sudo tee -a /etc/apt/sources.list.d/corretto.sources.list</code></p>
  <p><code>sudo apt update</code></p>
  <p><code>sudo apt install java-common java-11-amazon-corretto-jdk</code></p>
  <p><code>echo JAVA_HOME="/usr/lib/jvm/java-11-amazon-corretto" | sudo tee -a /etc/environment </code></p>
  <p><code>export JAVA_HOME="/usr/lib/jvm/java-11-amazon-corretto"</code></p>
  
  <li>Install Cassandra </li>
  <p><code>wget -qO -  https://downloads.apache.org/cassandra/KEYS | sudo gpg --dearmor  -o /usr/share/keyrings/cassandra-archive.gpg</code> </p>
  <p><code>echo "deb [signed-by=/usr/share/keyrings/cassandra-archive.gpg] https://debian.cassandra.apache.org 40x main" |  sudo tee -a /etc/apt/sources.list.d/cassandra.sources.list</code> </p>
  <p><code>sudo apt update</code> </p>
  <p><code>sudo apt install cassandra</code> </p>
  
  <li>Install ElasticSearch</li>
  <p> <code>wget -qO - https://artifacts.elastic.co/GPG-KEY-elasticsearch |  sudo gpg --dearmor -o /usr/share/keyrings/elasticsearch-keyring.gpg</code> </p>
  <p> <code>sudo apt-get install apt-transport-https</code> </p>
  <p> <code>echo "deb [signed-by=/usr/share/keyrings/elasticsearch-keyring.gpg] https://artifacts.elastic.co/packages/7.x/apt stable main" |  sudo tee /etc/apt/sources.list.d/elastic-7.x.list </code> </p>
  <p> <code>sudo apt update</code> </p>
  <p> <code>sudo apt install elasticsearch</code> </p>
  
  <li>Install TheHive </li>
  <p> <code>wget -O- https://archives.strangebee.com/keys/strangebee.gpg | sudo gpg --dearmor -o /usr/share/keyrings/strangebee-archive-keyring.gpg</code> </p>
  <p> <code>echo 'deb [signed-by=/usr/share/keyrings/strangebee-archive-keyring.gpg] https://deb.strangebee.com thehive-5.2 main' | sudo tee -a /etc/apt/sources.list.d/strangebee.list</code> </p>
 <p> <code>sudo apt-get update</code> </p>
  <p> <code> sudo apt-get install -y thehive</code> </p>
  
  <li>Check that TheHive web interface is accessible: http://<THEHIVE_IP>:9000</li>
    <li>Log in to TheHive </li>
    <p>Default login: admin@thehive.local </p>
    <p>Default password: secret</p>
</ol>
<h3>Integrate TheHive with Wazuh </h3>
Use the following blog post: https://wazuh.com/blog/using-wazuh-and-thehive-for-threat-protection-and-incident-response/
    
<h3>Create a custom rule to detect access to /etc/shadow, /etc/passwd and bash history </h3>
Follow the instructions here: https://wazuh.com/blog/hunting-for-linux-credential-access-attacks-with-wazuh/

<h3>Access credentials files to test the custom rule</h3>
<ol>
<li>Log in as a non-root user</li>
  <li>Run commands: cat /etc/passwd and cat /etc/shadow</li>
  <li>Go back to Wazuh Dashboard (Threat Hunting) and make sure the alert appears. Should look similar to the following:</li>
  <li>Check that the alerts also appear on TheHive</li>
</ol>

<h3>Set up Active Response for SSH Brute Force alert </h3>
Use the following manual: https://documentation.wazuh.com/current/user-manual/capabilities/active-response/ar-use-cases/blocking-ssh-brute-force.html

<h3>Test Active Response </h3>
<ol>
  <li>Use Metasploits module for SSH brute force </li>
  <p><code>msfconsole</code></p>
  <p><code>use scanner/ssh/ssh_login</code></p>
  <p><code>options</code> </p>
  <li>Configure options. For this one we need to set up RHOSTS, PASS_FILE or PASSWORD, USERNAME or USER_FILE. Other options are optional. In the end we should have something like this:</li>
  <li>On the separate window run ping command to check connection to the monitored host. Should be the same as in RHOSTS from the previous step</li>
  <li>Back in the Metasploit window start the attack</li>
  <code>exploit</code>
  <li>Check the Wazuh dashboard. Alerts should look similar to the following:</li>
  <li>Check TheHive Alerts Dashboard: </li>
</ol>












