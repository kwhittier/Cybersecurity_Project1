## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

Images/Project1_VirtualNetworkDiagram.pdf

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the install-elk.yml file may be used to install only certain pieces of it, such as Filebeat.


This document contains the following details:
- Description of the Topology
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly reliable, in addition to restricting access to the network. Load balancers protect the availability of your assets. A jump box interacts with the world so your individual machines don't have to - they stay protected by working through the jump box.


Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the files and system services.
- Filebeat collects data about the file system and forwards it to Logstash and Elasticsearch.
- Metricbeat records operating system/machine metrics and forwards them to Logstash and Elasticsearch.

The configuration details of each machine may be found below.


| Name     | Function      | IP Address | Operating System |
|----------|----------     |------------|------------------|
| Jump Box | Gateway       | 10.0.0.1   | Linux            |
| Web-1    | DVWA container| 10.0.0.7   | Linux            |                  |
| Web-2    | DVWA container| 10.0.0.8   | Linux            |
| Web-3    | DVWA container| 10.0.0.5   | Linux            |
| Elk-VM   | Monitoring    | 10.1.0.4   | Linux            |

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the jump box machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:
- <my home IP address>

Machines within the network can only be accessed by me.

- The jump box provisioner is allowed to access the ELK VM. Its most recent IP address was 40.112.217.53, but the IP address updates each time it is rebooted.

A summary of the access policies in place can be found in the table below.

| Name     | Publicly Accessible | Allowed IP Addresses |
|----------|---------------------|----------------------|
| Jump Box | Yes                 | varies               |
| Web-01   | No                  | <my home IP address> |
| Web-02   | No                  | <my home IP address> |
| Web-03   | No                  | <my home IP address> |
| ELK-VM   | No                  | <my home IP address> |

### ELK Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because automating configuration with Ansible  allows playbooks to be created and run on multiple boxes so they don't have to be installed one at a time.

The playbook implements the following tasks:
The steps of the ELK installation are:
- Configure ELK VM with docker
- Install python3-pip
- Install docker python module
- Increase memory
- Download and launch a docker ELK container

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

/Images/sudo_docker_ps_screenshot.png

### Target Machines & Beats
This ELK server is configured to monitor the following machines:
- 10.0.0.7
- 10.0.0.8
- 10.0.0.5

We have installed the following Beats on these machines:
- Filebeat
- Metricbeat

These Beats allow us to collect the following information from each machine:
- Filebeat will collect file data from each machine to monitor which files are changed and when. It organizes log files to send to Logstash and Elasticsearch.
- Metricbeat collects machine metrics, like uptime, and sends them to Logstash and Elasticsearch for easy analysis.

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- Copy the filebeat-playbook.yml file to a directory named "roles."
- Update the configuration (filebeat-config.cfg) file to include your ELK VM IP.
- Run the playbook, and navigate to Kibana ( <ELK external IP>/app/kibana ) to check that the installation worked as expected.
- Repeat steps with metricbeat-playbook.yml

_Summary Q&A:
- _Which file is the playbook? filebeat-playbook.yml
- _Where do you copy it? Directory called roles.
- _Which file do you update to make Ansible run the playbook on a specific machine? filebeat.cfg 
_ _How do I specify which machine to install the ELK server on versus which to install Filebeat on? By updating the IP address to match ELK IP address.
- _Which URL do you navigate to in order to check that the ELK server is running? http://413.77.149.144:5601/app/kibana

Use the following commands to download the playbook, update the files, etc.

$ cd /etc/ansible
$ mkdir files
$ git clone https://github.com/kwhittier/Cybersecurity_Project1
$ cp project-1/playbooks/* .
$ cp project-1/files/* ./files

Next, create a hosts files to run playbook on specific VMs by using:

$ cd /etc/ansible
$ cat > hosts <<EOF
<your webserver private IPs>

<your ELK VM private IP>
EOF

Then run the playbook by:
$ cd /etc/ansible
$ ansible-playbook install_elk.yml
$ ansible-playbook filebeat-playbook.yml
$ ansible-playbook metricbeat-playbook.yml

To see whether you've succeeded, wait five minutes then run: curl http://10.0.0.8:5601 (this is Kibana). You should see HTML if it works.