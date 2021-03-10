# Project-1-
## Automated ELK Stack Deployment


This document contains the following details:
- Description of the Topology
- Access Policies
- ELK Configuration
- Beats in Use
- Machines Being Monitored
- How to Use the Ansible Build


### Description of the Topology

- The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

- Load balancing ensures that the application will be highly available, in addition to restricting access to the network.

- What aspect of security do load balancers protect?
Load Balancing contributes to the Availability aspect (CIA Triad). 

What is the advantage of a jump box?
The advantage of a JumpBox is the orgination point for launching Administrative Tasks. Sets the JumpBox as a Secure Admin Workstation. When conducting any Administrative Task it is required to connect to the JumpBox before doing any task.

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the logs and system traffic.

- What does Filebeat watch for?
Watches for log files/locations and collects log events.

- What does Metricbeat record?
Records metric and statistical data from the operating system and from services running on the server.

The configuration details of each machine may be found below.

| Name       | IP Address | Usage        | OS                    |
|---         |---         |---           |---                    |
| Jump-Box-Provisioner    | 10.0.0.4   | Ansible      | Linux (ubuntu 18.04)  |
| VM1 | 10.0.0.9   | Docker-DVWA  | Linux (ubuntu 18.04)  |
| VM2   | 10.0.0.10  | Docker-DVWA  | Linux (ubuntu 18.04)  |
| VM3   | 10.0.0.7   | Docker-DVWA  | Linux (ubuntu 18.04)  |
| ELk-Server | 10.0.0.11  | Elk          | Linux (ubuntu 18.04)  |



### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the JumpBox machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:
-Personal IP Address

Machines within the network can only be accessed by SSH.
- The only machine that is able to connect to the Elk-Server (10.0.0.11) is via JumpBox from Private IP (10.0.0.4)

A summary of the access policies in place can be found in the table below.

| Name       | Publicly Accessible | Allowed IP Addresses   |
|----------  |---------------------|----------------------  |
| Jump-Box-Provisioner   | No                  | Personal IP Only       |
| VM1   | No                  | 10.0.0.4               |
| VM2   | No                  | 10.0.0.4               |
| VM3   | No                  | 10.0.0.4               |
| ELk-Server | No                  | 10.0.0.4 & Personal IP |

### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because...
- The main advantages of automating configuration through Ansible is ease of use and accessibility. Via Playbooks you can configure multiple Machines through a single command.

The playbook implements the following tasks:
- Create a New VM (should be named something simple "Elk-Server") Keep note of the Private IP (10.0.0.11) and the Public IP (0.0.0.0) you will need the Private IP to SSH into the VM and the Public IP to connect to the Kibana Portal (HTTP Site) to view all Metrics/Syslogs.
- Download and Configure the "elk-docker" container "In the hosts.conf you will need to add a new group [elkservers] and the Private IP (10.0.0.11) to the group. Then you need to create a new ansible-playbook ([elk.yml](https://uci.bootcampcontent.com/EmperorForgotten/elk-stack-project/blob/master/Ansible-Playbooks/elk.yml)) that will download, install, configures the "Elk-Server" to map the following ports [5601,9200,5044], and starts the container.
- Launch and expose the container "After installing and starting the new container. You can verify that the container is up and running by SSHing into the container from your JumpBox (SAW). Once you are in the [Elk-Server] run the command [sudo docker ps]
- Create new Inbound Security Rules to allow Ports: 5601 and 9200 "The Inbound Security Rules should allow access from your Personal Network"
- Open a new browser and type in the [Public IP:5601] to access the Kibana Portal Site



### Target Machines & Beats
This ELK server is configured to monitor the following machines:
- [10.0.0.7, 10.0.0.8, 10.0.0.9]

We have installed the following Beats on these machines:
- Filebeat and Metricbeat

These Beats allow us to collect the following information from each machine:
- Filebeat is a lightweight shipper for forwarding and centralizing log data. Filebeat monitors log files or locations you specify, collects log events, and forwards them either to Elasticsearch or Logstash for indexing.

- Metricbeat collects metrics from the operating system and from services running on the server. Metricbeat then takes the metrics and statistics that it collects and ships them to the output that you specify.

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- Copy the [filebeat.yml] and [metricbeat.yml] file to the /etc/ansible/roles/files/ directory.
- Update the configuration files to include the Private IP of the Elk-Server to the ElasticSearch and Kibana sections of the configuration file.
- Create a new ansible-playbook [filebeat-playbook.yml] in the /etc/ansible/roles/ directory that will install, drop in the [filebeat.yml] and [metricbeat.yml] files from the /etc/ansible/roles/files/ directory, uodate the configuration files, and start the service for both Filebeat and Metricbeat.
- Run the playbook, and navigate to ELk-Server to check that the installation worked as expected. [docker ps]

- Which file is the playbook? Where do you copy it? The playbook is called [filebeat-playbook.yml] You copy the file to the "/etc/ansible/hosts/" directory.
- 
- Which file do you update to make Ansible run the playbook on a specific machine? How do I specify which machine to install the ELK server on versus which to install Filebeat on? The file you need to update is the file which is a configuration file that will be dropped into the Elk-Server during the run of the ansible-playbook. When you update the host.cfg file in the ansible directory you will need to create a new group called [elkservers] and add the Private IP of the Elk-Server to the group. Then when configuring the [filebeat.yml] file you need to designate the Private IP of the Elk-Server in two lines of the .yml file. Lines 1106 and 1806 are the needed to be updated with the Private IP.

- Which URL do you navigate to in order to check that the ELK server is running? The URL to use to verify the Elk-Server is running is the Public IP (0.0.0.0:5601)

The commands needed to run the Ansible configuration for the Elk-Server are:
- ssh azaadmin@JumpBox(PrivateIP)
- sudo docker container list -a (locate your ansible container)
- sudo docker start container (name of the container)
- sudo docker attach container (name of the container)
- cd /etc/ansible/
- ansible-playbook elk.yml (configures Elk-Server and starts the Elk container on the Elk-Server) wait a couple minutes for the implementation of the Elk-Server
- cd /etc/ansible/roles/
- ansible-playbook filebeat-playbook.yml (installs Filebeat and Metricbeat)
- open a new web browser (Elk-Server PublicIP:5601) This will bring up the Kibana Web Portal

You will need to ensure all files are properly placed before running the ansible-playbooks.
