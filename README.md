# SOC-Project

## Objective
In this lab, we will explore the practical aspects of setting up a Security Operations Center (SOC) home lab. Our focus will be on understanding and implementing Security Orchestration, Automation, and Response (SOAR) solutions.  We will gain hands-on experience with real-world scenarios, enhancing our skills as SOC analysts.

# Building a Home Lab for SOC Analyst Learning: A Journey into SOAR Solutions 🚀

I'm thrilled to share my latest project—an experimental learning endeavor focused on **SOC (Security Operations Center)** analysis. In this project, we'll explore the architecture and components of a home lab designed for hands-on experience with security event management, orchestration, and automated threat response.

## Lab Structure

1. **Windows 10 Agents/Users**:
   - These agents run **Wazuh** to generate security events.
   - Their role is crucial in simulating real-world scenarios.

2. **Wazuh Manager Server**:
   - The central hub for collecting and managing security events from Windows 10 agents.
   - It acts as the nerve center of our SOC setup.

3. **Shuffle Integration**:
   - Orchestrates the flow of data—from event collection to response actions.
   - Shuffle streamlines the entire process, ensuring efficient incident handling.

4. **The Hive**:
   - A powerful tool for centralizing alerts and managing incident response.
   - Integrates seamlessly with Shuffle, allowing automated OSINT (Open Source Intelligence) gathering.

5. **SOC Analyst Station**:
   - My workspace for monitoring and responding to alerts.
   - As an analyst, I play a critical role in assessing and mitigating security incidents.

## Data Flow

1. **Windows 10 → Wazuh Manager**:
   - Security events originate on Windows 10 machines.
   - Wazuh agents forward these events to the Wazuh Manager.

2. **Wazuh Manager → Shuffle**:
   - The Wazuh Manager sends events to Shuffle for orchestration.
   - Shuffle coordinates the incident response workflow.

3. **Shuffle → The Hive**:
   - Automated OSINT tools within The Hive gather additional context.
   - Alerts are centralized, providing a comprehensive view of incidents.

4. **Shuffle → SOC Analyst**:
   - When an alert triggers, an email notifies me—the SOC analyst.
   - I promptly assess the situation and take necessary actions.

5. **SOC Analyst → Shuffle**:
   - I respond to the alert, performing investigations or mitigation steps.
   - Shuffle records my actions for future reference.

6. **Shuffle → Wazuh Manager**:
   - Response actions are communicated back to the Wazuh Manager.
   - These actions may include blocking an IP address, quarantining a host, or other protective measures.

7. **Wazuh Manager → Windows 10**:
   - The Wazuh Manager executes response actions on the Windows 10 endpoints.
   - This completes the incident lifecycle.

## Lab Components in the Cloud

Our lab environment consists of:

- **Two Cloud Servers**:
  - One hosts the Wazuh Manager.
  - The other hosts Shuffle.
- **Virtual Machine with Windows 10**:
  - Simulates the Windows 10 environment.
- **SOC Analyst Station**:
  - My workspace for active monitoring and incident handling.

Feel free to explore this setup, adapt it to your needs, and embark on your own SOC learning journey! 🌟

---

### Wazuh Installation

1. **SSH into Wazuh Server:**
   ```bash
   ssh root@<Public IP of Wazuh>
   ```

2. **Update and Upgrade:**
   ```bash
   apt-get update && apt-get upgrade
   ```

3. **Install Wazuh 4.7:**
   ```bash
   curl -sO https://packages.wazuh.com/4.7/wazuh-install.sh && sudo bash ./wazuh-install.sh -a
   ```

4. **Extract Wazuh Credentials:**
   ```bash
   sudo tar -xvf wazuh-install-files.tar
   ```

5. **Wazuh Dashboard Credentials:**
   - **User:** admin
   - **Password:** ***************

6. **Access Wazuh Dashboard:**
   - Open your browser and go to: `https://<Public IP of Wazuh>`

---

### TheHive Installation

1. **SSH into TheHive Server:**
   ```bash
   ssh root@<Public IP of TheHive>
   ```

2. **Update and Upgrade:**
   ```bash
   apt-get update && apt-get upgrade
   ```

3. **Install Dependencies:**
   ```bash
   apt install wget gnupg apt-transport-https git ca-certificates ca-certificates-java curl  software-properties-common python3-pip lsb-release
   ```

4. **Install Java:**
   ```bash
   wget -qO- https://apt.corretto.aws/corretto.key | sudo gpg --dearmor  -o /usr/share/keyrings/corretto.gpg
   echo "deb [signed-by=/usr/share/keyrings/corretto.gpg] https://apt.corretto.aws stable main" |  sudo tee -a /etc/apt/sources.list.d/corretto.sources.list
   sudo apt update
   sudo apt install java-common java-11-amazon-corretto-jdk
   echo JAVA_HOME="/usr/lib/jvm/java-11-amazon-corretto" | sudo tee -a /etc/environment 
   export JAVA_HOME="/usr/lib/jvm/java-11-amazon-corretto"
   ```

5. **Install Cassandra:**
   ```bash
   wget -qO -  https://downloads.apache.org/cassandra/KEYS | sudo gpg --dearmor  -o /usr/share/keyrings/cassandra-archive.gpg
   echo "deb [signed-by=/usr/share/keyrings/cassandra-archive.gpg] https://debian.cassandra.apache.org 40x main" |  sudo tee -a /etc/apt/sources.list.d/cassandra.sources.list
   sudo apt update
   sudo apt install cassandra
   ```

6. **Install ElasticSearch:**
   ```bash
   wget -qO - https://artifacts.elastic.co/GPG-KEY-elasticsearch |  sudo gpg --dearmor -o /usr/share/keyrings/elasticsearch-keyring.gpg
   sudo apt-get install apt-transport-https
   echo "deb [signed-by=/usr/share/keyrings/elasticsearch-keyring.gpg] https://artifacts.elastic.co/packages/7.x/apt stable main" |  sudo tee /etc/apt/sources.list.d/elastic-7.x.list
   sudo apt update
   sudo apt install elasticsearch
   ```

7. **Install TheHive:**
   ```bash
   wget -O- https://archives.strangebee.com/keys/strangebee.gpg | sudo gpg --dearmor -o /usr/share/keyrings/strangebee-archive-keyring.gpg
   echo 'deb [signed-by=/usr/share/keyrings/strangebee-archive-keyring.gpg] https://deb.strangebee.com thehive-5.2 main' | sudo tee -a /etc/apt/sources.list.d/strangebee.list
   sudo apt-get update
   sudo apt-get install -y thehive
   ```

8. **Default Credentials for TheHive:**
   - **Port:** 9000
   - **Credentials:** 'admin@thehive.local' with a password of 'secret'

## Configuration for TheHive

### Configure Cassandra

1. **Edit Cassandra Config File:**
   ```bash
   nano /etc/cassandra/cassandra.yaml
   ```

2. **Change Cluster Name:**
   ```yaml
   cluster_name: 'jashan'
   ```

3. **Update Listen Address:**
   ```yaml
   listen_address: <public IP of TheHive>
   ```

4. **Update RPC Address:**
   ```yaml
   rpc_address: <public IP of TheHive>
   ```

5. **Update Seed Provider:**
   ```yaml
   - seeds: "<Public IP Of the TheHive>:7000"
   ```

6. **Stop Cassandra Service:**
   ```bash
   systemctl stop cassandra.service
   ```

7. **Remove Old Files:**
   ```bash
   rm -rf /var/lib/cassandra/*
   ```

8. **Restart Cassandra Service:**
   ```bash
   systemctl start cassandra.service
   ```

### Configure ElasticSearch

1. **Edit ElasticSearch Config File:**
   ```bash
   nano /etc/elasticsearch/elasticsearch.yml
   ```

2. **Update Cluster Name and Host:**
   ```yaml
   cluster.name: thehive
   node.name: node-1
   network.host: <Public IP of your TheHive instance>
   http.port: 9200
   ```

3. **Start ElasticSearch Service:**
   ```bash
   systemctl start elasticsearch
   systemctl enable elasticsearch
   systemctl status elasticsearch
   ```

## Configure TheHive

1. **Ensure Proper Ownership:**
   ```bash
   ls -la /opt/thp
   chown -R thehive:thehive /opt/thp
   ```

2. **Edit TheHive Configuration File:**
   ```bash
   nano /etc/thehive/application.conf
   ```

3. **Update Database and Index Configuration:**
   ```conf
   db.janusgraph {
     storage {
       backend = cql
       hostname = ["<Public IP of TheHive>"]
       cql {
         cluster-name = jashan
         keyspace = thehive
       }
     }
   }

   index.search {
     backend = elasticsearch
     hostname = ["<Public IP of TheHive>"]
     index-name = thehive
   }

   application.baseUrl = "http://<Public IP of TheHive>:9000"
   ```

4. **Start TheHive Services:**
   ```bash
   systemctl start thehive
   systemctl enable thehive
   systemctl status thehive
   ```

## Configuration on Wazuh

### Retrieve Wazuh Credentials

1. **Access Wazuh Console and Extract Credentials:**
   ```bash
   tar -xvf wazuh-install-files.tar
   cd wazuh-install-files/
   cat wazuh-passwords.txt
   ```

2. **Add Windows 10 Agent to Wazuh:**
   - Click on "Add Agent" and select "Windows" as package.
   - Enter Wazuh Public IP as server address.
   - Assign an agent name.
   - Copy the provided command and run it on the Windows 10 machine.
   - Start Wazuh agent service:
     ```bash
     net start wazuhsvc
     ```

3. **Verify Agent Activation:**
   - After a few minutes, the Windows 10 agent should appear as active in the Wazuh Dashboard.

---



