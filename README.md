# ELK-Stack-Project
## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

![image](https://github.com/UCB-CyberSecurity-Cohort5/elk-stack-project-NMM4/blob/main/Network%20Diagram%20w.%20Elk.drawio.png)

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the playbook file may be used to install only certain pieces of it, such as Filebeat.

![image](https://github.com/UCB-CyberSecurity-Cohort5/elk-stack-project-NMM4/blob/main/playbook/filebeat-playbook.yml.png)

This document contains the following details:
- Description of the Topologu
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly available, in addition to restricting access to the network.

- _What aspect of security do load balancers protect?_
- _Load Balancers protects organizations against distributed DDoS (denial-of-service) attacks by utilizing the offloading function, allowing you to distribute network traffic evenly to prevent failure that's caused by overloading a particular resource._ 

_What is the advantage of a jump box?_
- _The advantage of a jump box is that it can restrict access of your virtual network to the public. An individual would need the private IP addresses of the machines to access the other virtual machines and this is how a jumpbox can control access._

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the log files and system traffic.

- _Filebeat's function is to primarily watch for system logs and forward any changes to the elasticsearch host_
- _Metricbeat collects metric from the operating system and from services running on the server and sends them to elasticsearch, which can visualize the data.  

The configuration details of each machine may be found below.
| Name                   | Function       | IP Address | Operating System |
|------------------------|----------------|------------|------------------|
| Jump-Box-Provisioner   | Gateway        | 10.0.0.4   | Linux            |
| Web-1                  | Webserver      | 10.0.0.5   | Linux            |
| Web-2                  | Webserver      | 10.0.0.6   | Linux            |
| Web-3                  | Webserver      | 10.0.0.7   | Linux            |
| ELK-SERVER             | Elasticsearch  | 10.1.0.6   | Linux            |
### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the Jump-Box-Provisioner machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:
- _My public IP address: 23.114.183.98_

Machines within the network can only be accessed by the Jump-Box-Provisioner virtual machine.
- _Which machine did you allow to access your ELK VM? What was its IP address?_
- _Jump-Box-Provisioner: 13.64.49.179_

A summary of the access policies in place can be found in the table below.

| Name          | Publicly Accessible | Allowed IP Addresses |
|---------------|---------------------|----------------------|
| Jump Box      | Yes/No              | 13.64.49.179         |
| Web-1         | No                  | 10.0.0.4             |
| Web-2         | No                  | 10.0.0.4             |
| Web-3         | No                  | 10.0.0.4             | 
| Load Balancer | Yes, Port 80        | 13.64.49.179         |  
| ELK-SERVER    | Yes, Port 5601      | 13.64.49.179         |  

### Elk Configuration
-_What is the main advantage of automating configuration with Ansible?_
Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because it minimizes configuration errors.

The playbook implements the following tasks:
- _First, install docker.io to the remote server_
- _Second, install python3_pip_
- _Third, since Elk required more memory, increased the virtual memory_
- _Fourth, download and launch the Elk container, which downloads the Elk docker container_

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

![TODO: Update the path with the name of your screenshot of docker ps output](Images/docker_ps_output.png)

### Target Machines & Beats
This ELK server is configured to monitor the following machines:
- _List the IP addresses of the machines you are monitoring_
- _Web-1: 10.0.0.5_
- _Web-2: 10.0.0.6_
- _Web-3: 10.0.0.7_

We have installed the following Beats on these machines:
- _Specify which Beats you successfully installed_
- _Filebeat_
- _Metricbeat_

These Beats allow us to collect the following information from each machine:
- _What kind of data does each beat collect?_
- _Filebeat collects log information about the file system and specifies which files have been changed. If changes in files have been detected, they are send to elasticsearch. The output can be analyzed via Kibana. 
- _Metricbeat visualizes data on every process that is currently running on your system, including memory, CPU, file syste, and more. To do this via Kibana, ensure that your VM is running, navigate to Kibana and select the system you'd like to investigate._

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- Copy the filebeat-config-yml file to /etc/ansible/files/filebeat-config.yml.
- Update the filebeat-config.yml file to include host 10.1.0.6:5601"
- Run the playbook, and navigate to Kibana to check that the installation worked as expected.
- _Run the playbook: ansible-playbook /etc/ansible/roles/filebeat-playbook.yml
- _Navigate to Kibana: http://20.25.100.30:5601/app/kibana#_

- _Which file is the playbook? Where do you copy it?_
- _The playbook file is: filebeat-playbook.yml and it's copied to /etc/ansible/roles/filebeat-playbook.yml_

- _Which file do you update to make Ansible run the playbook on a specific machine? How do I specify which machine to install the ELK server on versus which to install Filebeat on?_


- _Which URL do you navigate to in order to check that the ELK server is running?_
http://20.25.100.30:5601/app/kibana#

_**Bonus*: Provide the specific commands the user will need to run to download the playbook, update the files, etc._
1. <nano filebeat-config.yml>
   scroll down to IP and user name to change the IP address to my Elk server: 10.1.0.6:9200, 
   scroll further down to line 1806 to modify the IP address to reflect the Elk Server IP address, save and exit. 
   run <ansible-playbook /etc/ansible/roles/filebeat-playbook.yml>
   If output contains "ok" messages and no errors, filebeat has been successfully downloaded. 
2. <nano metricbeat-playbook.yml>
   modify the yaml file to include the correct location of the metricbeat-config.yml and metricbeat.yml files, save and exit.
   run <ansible-playbook /etc/ansible/roles/metricbeat-playbook.yml>
   If output contains "ok" messages and no errors, metricbeat has been successfully downloaded. 

