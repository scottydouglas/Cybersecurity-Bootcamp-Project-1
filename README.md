## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

![ELK Stack Diagram](https://github.com/scottydouglas/Cybersecurity-Bootcamp-Project-1/blob/main/Images/ELK%20Stack%20Diagram.png)

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the playbook file may be used to install only certain pieces of it, such as Filebeat.

### Resources

Ansible Playbooks

- [Elk](https://github.com/scottydouglas/Cybersecurity-Bootcamp-Project-1/blob/main/Ansible/elk.yml)
- [DVWA](https://github.com/scottydouglas/Cybersecurity-Bootcamp-Project-1/blob/main/Ansible/dvwa.yml)
- [Filebeat](https://github.com/scottydouglas/Cybersecurity-Bootcamp-Project-1/blob/main/Ansible/filebeat.yml)
- [Metricbeat](https://github.com/scottydouglas/Cybersecurity-Bootcamp-Project-1/blob/main/Ansible/metricbeat.yml)

This document contains the following details:
- Description of the Topology
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the Damn Vulnerable Web Application.

Load balancing ensures that the application will be highly reliable, in addition to restricting any potential harmful or unwanted traffic to the network.

- Load Balancers provided more reliability to your web apps by using a firewall. They also provide insurance against Denial of Service attacks by ensuring each machine on the network receives equal amounts of traffic. 

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the network logs and system traffic.
- Filebeat monitors logs events across a network and can be used to both monitor and archive any pertinent network activity.
- Metricbeat monitors system diagnostic information such as CPU and Memory Usage. These metrics are monitored across the network and through any active containers and services.

The configuration details of each machine may be found below.

| Name       | Function    | IP Address               | Operating System |
|------------|-------------|--------------------------|:----------------:|
| Jump Box   | Gateway     | 13.64.99.99 / 10.0.0.5   | Linux            |
| Web-1      | Application | 10.0.0.4                 | Linux            |
| Web-2      | Application | 10.0.0.6                 | Linux            |
| ELK Server | Logging     | 23.101.154.44 / 10.1.0.4 | Linux            |

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the Jump Box machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:

- 24.212.205.42

Machines within the network can only be accessed through SSH connection within the ansible container.

- The only way to access my ELK VM is through the Jump Box. 

A summary of the access policies in place can be found in the table below.

| Name          | Public Access | Whitelisted IP Addresses |
|---------------|---------------|--------------------------|
| Jump Box      | Yes           | 24.212.205.42            |
| Web-1         | No            | 10.0.0.5                 |
| Web-2         | No            | 10.0.0.5                 |
| ELK Server    | Yes           | 24.212.205.42            |
| Load Balancer | Yes           | 10.0.0.5 / 24.212.205.42 |

### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because Ansible allows users the ability to provision, configure and deploy many systems in a shorter amount of time. Ansible is a good example of Infrastructure as Code.

The playbook implements the following tasks:
- Install Docker
- Install Python pip3
- Install Docker container
- Increase total system virtual memory
- Install ELK container

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

![Docker PS - ELK instance](https://github.com/scottydouglas/Cybersecurity-Bootcamp-Project-1/blob/main/Images/elk%20docker%20ps.PNG)

### Target Machines & Beats
This ELK server is configured to monitor the following machines:
- Web-1; IP: 10.0.0.4
- Web-2; IP: 10.0.0.6

We have installed the following Beats on these machines:
- Filebeat
- Metricbeat

These Beats allow us to collect the following information from each machine:
- Filebeat
  - Filebeat is a program that helps track log data across a network. Logs track significant actions that occur across a network, such as user logins and SSH attempts.
- Metricbeat
  - Metricbeat tracks system-level CPU usage, memory, file system, disk IO, and network IO statistics.

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- Copy the filebeat-config.yml file to /etc/ansible.

- Update the filebeat-config.yml file to include:	
  - An updated username and password
  - Replace the IP address with the IP address of your ELK server
  - Output.elasticsearch:
  - hosts: ["10.1.0.4:9200"]
  - username: "elastic"
  - password: "changeme"

- Go to line #1806 and replace the IP address with the IP address of your ELK machine/kibana port.
  - Setup.kibana:
  - host: "10.1.0.4:5601"
  - save this file to /etc/ansible/filebeat-config.yml

- Run the playbook, and navigate to kibana to check that the installation worked as expected.

- The file titled "Filebeat" under the "Ansible Playbooks" section of this document is the playbook file. This will need to be placed in the /etc/ansible/ folder of your Ansible container with the current configuration.

- In order to ensure Ansible runs the Filebeat playbook on a specific machine, the 'hosts' config file must be updated. It can be located in the etc/ansible folder in the Ansible container. In this config file, add any IP addresses to grant playbook access to specific machines. In order to complete the setup, change the host name in the playbook.

- Navigate to http://23.101.154.44:5601/app/kibana to ensure your ELK server is operational.
