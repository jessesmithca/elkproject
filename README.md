# uCSd
Repository for UCSD Cybersecurity Course

## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

![ElkDiagram](https://raw.githubusercontent.com/jessesmithca/uCSd/main/Diagrams/ElkDiagram.png)

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the playbook file may be used to install only certain pieces of it, such as Filebeat.

![filebeatplaybook](https://raw.githubusercontent.com/jessesmithca/uCSd/main/Ansible/Images/filebeat_playbook_yml.png)

This document contains the following details:
- Description of the Topologu
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the Damn Vulnerable Web Application.

----
##### Load Balancing
Load balancing ensures that the application will be highly stable, in addition to restricting access to the network. A load balancer protects against things like DDoS attacks by directing traffic flowing torwards a server to a cloud made up of multiple machines.
----

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the system metrics and system logs.

----
Filebeat watches for "event data" in log files specified by the user. In this case, it is configured to monitor system logs for changes and report them to elasticsearch, which is then formatted by kibana.
 
Metricbeat monitors system metrics and running services. It records metrics from a variety of services, some examples include Apache, MySQL, and System.
----

The configuration details of each machine may be found below.
_Note: Use the [Markdown Table Generator](http://www.tablesgenerator.com/markdown_tables) to add/remove values from the table_.

| Name     | Function | IP Address | Operating System |
|----------|----------|------------|------------------|
| Jump Box | Gateway  | 10.0.0.4   | Linux            |
| Web1     | Container| 10.0.0.5   | Linux            |
| Web2     | Container| 10.0.0.6   | Linux            |
| Elk      | Kibana   | 10.1.0.4   | Linux            |

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the Jump Box machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:

		72.207.73.190

Machines within the network can only be accessed by users with the appropriate ssh key on the local network.
- Which machine did you allow to access your ELK VM? 
	
		The Docker Container on the Jump Box.
	
- What was its IP address?
	
		13.88.181.4
		10.0.0.4

A summary of the access policies in place can be found in the table below.

| Name     | Publicly Accessible | Allowed IP Addresses |
|----------|---------------------|----------------------|
| Jump Box | Yes                 |    72.207.73.190     |
| Web 1    | No                  |                      |
| Web 2    | No                  |                      |
| Elk      | No                  |                      |


### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because...
- What is the main advantage of automating configuration with Ansible?_

		To quote ansible directly: "With Ansible, IT admins can begin automating away the drudgery from their daily tasks." Many aspects of IT admin and other adjacent jobs potentially include a great deal of repetetive tasks. 
		Using ansible would allow someone to rapidly complete those repetetive tasks rather than tediously do each step manually.
		Additionally, ansibile is designed to be human-readable, which would potentially allow for someone to complete jobs they otherwise might struggle to complete.

The playbook implements the following tasks:
- In 3-5 bullets, explain the steps of the ELK installation play. E.g., install Docker; download image; etc._
		
		1. Add the elk server to the appropriate hosts file
		2. Create ansible playbook that installs Docker, configures the container by installing and setting up relevant packages like filebeat, metricbeat, etc.
		3. Run the playbook using the ansible-playbook command to deploy the image to the target machine(s)
		4. Access kibana using http:// (elk machine IP):5601/app/kibana

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

![dockerps](https://raw.githubusercontent.com/jessesmithca/uCSd/main/Ansible/Images/elk_docker_ps.png)

### Target Machines & Beats
This ELK server is configured to monitor the following machines:

		10.0.0.5 (Web1 VM)
		10.0.0.6 (Web2 VM)

We have installed the following Beats on these machines:
- Specify which Beats you successfully installed_

		Filebeat and Metricbeat

These Beats allow us to collect the following information from each machine:
- In 1-2 sentences, explain what kind of data each beat collects, and provide 1 example of what you expect to see. E.g., `Winlogbeat` collects Windows logs, which we use to track user logon events, etc._

		Filebeat monitors system log files (or other files as the user specifies) for log events, forwarding what it collects to Elasticsearch.
			I would expect to see things like changes made to log files. For example, a change made to /var/log/auth.log might include an ssh authentication failure as well as other useful data associated with the failed authentication, like the IP address, geo location, city, etc.
		Metricbeat collects system metrics.
			I would expect to see system metrics like statistics about memory usage/availability, cpu info, OS versions, kernel information, docker container information, etc.

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
		- Copy the filebeat-config.yml file to /etc/ansible/files.
		- Update the filebeat-config.yml file to include the IP of your Elk machine on lines 1105 and 1805
		- Run the playbook, and navigate to http://(YOUR_ELK_MACHINE_IP):5601/app/kibana to check that the installation worked as expected.

Answer the following questions to fill in the blanks:
- Which file is the playbook? Where do you copy it?

		The playbook is filebeat-playbook.yml and you should copy it to /etc/ansible/roles

- Which file do you update to make Ansible run the playbook on a specific machine? How do I specify which machine to install the ELK server on versus which to install Filebeat on?

		By updating the hosts file located in /etc/ansible/hosts - make sure you have the elk machine listed as its own group. 
		There should be a [webservers] group with your Web1 and Web2 machines ip addresses in it, and an [elk] group with your elk machine's ip in it, all followed by ansible_python_interpreter=/usr/bin/python3.
		The filebeat-playbook.yml says hosts: webservers on the 3rd line, indicating that it is being applied to all machines in the [webservers] group in the hosts file.

- Which URL do you navigate to in order to check that the ELK server is running?
		
		http://(YOUR_ELK_MACHINE_IP):5601/app/kibana

As a **Bonus**, provide the specific commands the user will need to run to download the playbook, update the files, etc.

To download the playbook, use 
		
		curl -L -O https://github.com/jessesmithca/uCSd/blob/main/Ansible/YML/filebeat-playbook.yml
		
To download the filebeat-config.yml, use
		
		curl -L -O https://github.com/jessesmithca/uCSd/blob/main/Ansible/YML/filebeat-config.yml
		
To download the metricbeat.yml, use

		curl -L -O https://github.com/jessesmithca/uCSd/blob/main/Ansible/YML/metricbeat.yml
