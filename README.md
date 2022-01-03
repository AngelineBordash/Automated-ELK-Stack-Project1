# Automated-ELK-Stack-Project1


Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.
![Diagram](https://user-images.githubusercontent.com/89946160/147896467-579e4440-a89e-4e97-8afb-a1bc0055bcc4.jpg)

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the all-plays-combined.yml file may be used to install only certain pieces of it, such as Filebeat.
[all_plays_combined.txt](https://github.com/AngelineBordash/Automated-ELK-Stack-Project1/files/7800181/all_plays_combined.txt)

This document contains the following details:
•	Description of the Topologu
•	Access Policies
•	ELK Configuration
•	Beats in Use
•	Machines Being Monitored
•	How to Use the Ansible Build

Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly reliable, in addition to restricting access to the network.
•	Load balancers protect the network from a Denial of Service (DoS) attack by distributing traffic evenly among the servers. The jump box gives the ability to have only one machine within secure networks that can access the servers. There is also the added advantage of it being a place from which all configurations can be made securely for all machines within all relative networks.

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the data and system logs.
•	Filebeat watches for and collects log data, as well as new log content, from the log files.
•	Metricbeat records the metrics collected from both the system and the services running on the server.

The configuration details of each machine may be found below.
Name	Function	IP Address	Operating System
			
Jump Box 	Gateway	10.0.0.4	Linux Unbuntu 18.4
Web1	Server	10.0.0.5	Linux Unbuntu 18.4
Web2	Server	10.0.0.6	Linux Unbuntu 18.4
ELK	Monitor	10.2.0.4	Linux Unbuntu 18.4

Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the ELK and Jumpbox machines can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:
•	75.190.44.1

Machines within the network can only be accessed by the Jumpbox (IP address 10.0.0.4), including the ELK machine.
A summary of the access policies in place can be found in the table below.

Name	Publicly Accessible	Allowed IP Addresses
		
Jumpbox	No	75.190.44.1
Web1	No	75.190.44.1, 10.0.0.4, 10.2.0.4
Web2	No	75.190.44.1, 10.0.0.4, 10.2.0.4
ELK	No	75.190.44.1, 10.2.0.4

Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because IaC allows configurations to be performed automatically on all machines instantaneously. This is extremely efficient in time and money because any necessary corrections can be made at one time.
The playbook implements the following tasks:
•	Install Docker
•	Install Python3
•	Increase virtual memory
•	Download image: sebp/elk:761
•	Enable service docker on boot
The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.
![Elk_sebp761](https://user-images.githubusercontent.com/89946160/147896606-0b733036-b213-471b-8ea8-55aa6a6843eb.png)

Target Machines & Beats
This ELK server is configured to monitor the following machines:
•	10.0.0.5
•	10.0.0.6

We have installed the following Beats on these machines:
•	Filebeat
•	Metricbeat

These Beats allow us to collect the following information from each machine:
Filebeat collects data and log events, as well as log files. Filebeat uses a harvester to gather new log data and then sends any new content to Elasticsearch, which is accessible by the ELK server through Kibana.
Metricbeat collects metrics on the systems and services running on the servers. This data is then sent to Elasticsearch from the ELK server through Kibana.

Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 
SSH into the control node and follow the steps below:
1.	Download Filebeat and Metricbeat .deb files onto your Ansible container.
2.	Run <curl https://gist.githubusercontent.com/slape/5cc350109583af6cbe577bbcc0710c93/raw/eca603b72586fbe148c11f9c87bf96a63cb25760/Filebeat > /etc/ansible/filebeat-config.yml>
3.	Scroll to line 1106 and 1806 in this configuration file and change the IP to match the IP of your ELK server.
4.	Run <curl https://gist.githubusercontent.com/slape/58541585cc1886d2e26cd8be557ce04c/raw/0ce2c7e744c54513616966affb5e9d96f5e12f73/metricbeat > /etc/ansible/metricbeat-config.yml>
•	Change the IP addresses in this file similarly to the  filebeat-config.yml file wherever necessary.
5.	Run <curl https://raw.githubusercontent.com/JaniceEstes/Cybersecurity_Project_1/main/Ansible/all_plays_combined.txt > /etc/ansible/roles/all_plays_combined.yml>
6.	Update the /etc/ansible/hosts file to include:
•	< [IP address of webserver] ansible_python_interpreter=/usr/bin/python3 >
•	Follow this step for each webserver to be configured
•	Create a separate group for ELK within the hosts configuration file and add the IP address of the ELK server and add < ansible_python_interpreter=/usr/bin/python3 >
7.	Add the username for your ELK machine in the ansible.cfg under "remote users" and to the playbook.
8.	Run the playbook, and navigate to [http://{IP of your ELK server}/app/kibana#/home/tutorial/systemLogs], scroll to the bottom and click "Check Data" to check that the Filebeat installation worked as expected. Then navigate to [http://{IP of your ELK server}/app/kibana#/home/tutorial/systemMetrics], scroll to the bottom and click "Check Data" to check that the Metricbeat installation worked as expected.
Commands the user will need to download and run the playbook and update the files successfully.
1.	SSH into your Jump Box from your personal workstation/physical machine (this is the only machine that should have access to your Jump Box): ssh username@ipaddress
2.	To SSH into your Ansible container, run: sudo docker ps
•	You will see the name of your Ansible container (e.g. fervent_satoshi)
3.	Run:
•	sudo docker start [name of your ansible container]
•	sudo docker attach [name of your ansible container]
•	Here is an example of what this looks like:
![DockerClip](https://user-images.githubusercontent.com/89946160/147896708-50a8ef87-cbf4-4b9e-bc78-f0dbbaa10bfc.PNG)
4.	From Ansible root, run:
•	cd /etc/ansible
•	mkdir roles
•	mkdir files
5.	Download filebeat onto ansible by running: 
•	curl -L -O https://artifacts.elastic.co/downloads/beats/filebeat/filebeat-7.6.1-amd64.deb > /etc/ansible/roles/filebeat-7.6.1-amd64.deb
6.	Download metricbeat onto ansible by running: 
1.	curl -L -O https://artifacts.elastic.co/downloads/beats/metricbeat/metricbeat-7.6.1-amd64.deb > /etc/ansible/roles/metricbeat-7.6.1-amd64.deb
7.	Download the filebeat-config.yml file by running:
1.	curl https://gist.githubusercontent.com/slape/5cc350109583af6cbe577bbcc0710c93/raw/eca603b72586fbe148c11f9c87bf96a63cb25760/Filebeat > /etc/ansible/files/filebeat-config.yml
8.	Download the metricbeat-config.yml file by running:
1.	curl https://gist.githubusercontent.com/slape/58541585cc1886d2e26cd8be557ce04c/raw/0ce2c7e744c54513616966affb5e9d96f5e12f73/metricbeat > /etc/ansible/files/metricbeat-config.yml
9.	Run the following to edit configuration files:
1.	cd files
2.	nano filebeat-config.yml
3.	nano metricbeat-config.yml
10.	Go through each -config.yml file and update the IP addresses to match the ELK server.
11.	Download the playbook by running:
1.	curl https://raw.githubusercontent.com/JaniceEstes/Cybersecurity_Project_1/main/Ansible/all_plays_combined.txt > /etc/ansible/roles/all_plays_combined.yml
12.	You must update the hosts file, run:
1.	cd /etc/ansible
2.	nano hosts
•	Under [webservers], include < [IP address of webserver] ansible_python_interpreter=/usr/bin/python3 >
•	Follow this step for each webserver to be configured
•	Create a group for elk below the webservers group within the hosts file and add the IP address of the ELK server
•	Refer to this screenshot for guidance:
![ConfigFile](https://user-images.githubusercontent.com/89946160/147896761-1f4ac5c1-15e3-462e-b2c9-db4a872daa0f.png)
13.	Make sure the username for your ELK machine is found within /etc/ansible/ansible.cfg under "remote users".
•	nano ansible.cfg
•	Here is what the ansible.cfg file looks like:
![username config](https://user-images.githubusercontent.com/89946160/147896821-6b8bb64f-3d85-4e0a-b419-7d92b5b78359.PNG)
14.	Update the playbook to include the username(s) that match your webservers and your ELK server.
•	nano all_plays_combined.yml
•	Double check that all hosts and remote users match your machines. You will need to scroll to check this thoroughly.
•	Refer to this screenshot for guidance on where to change the "remote user" name to your own for the ELK server: 
![PlaybookYML](https://user-images.githubusercontent.com/89946160/147896853-98365711-56d7-4007-b500-b191c70cc794.PNG)
15.	Run the playbook with the following commands:
•	cd /etc/ansible/roles
•	ansible-playbook all_plays_combined.yml
•	This is what it looks like when the playbook runs successfully:
![MetricBeatInstall](https://user-images.githubusercontent.com/89946160/147896903-e744be18-08f3-4266-849b-b8d9d770dac4.PNG)
16.	Navigate to <http://[IP of your ELK server]/app/kibana#/home/tutorial/systemLogs> scroll to the bottom of the page, and click "Check Data" to verify the Filebeat installation was successful.
•	Here is what it looks like:
<img width="940" alt="filebeat data successful" src="https://user-images.githubusercontent.com/89946160/147896942-f21059f8-47cd-41c7-a29a-3d096633ae7b.png">
17.	Then navigate to <http://[IP of your ELK server]/app/kibana#/home/tutorial/systemMetrics> scroll to the bottom and click "Check Data" to verify the Metricbeat installation was successful. 
•	Here is what it looks like:
<img width="960" alt="metricbeat data successful" src="https://user-images.githubusercontent.com/89946160/147896961-7330871f-5759-488c-b7e5-0f37cdb1ce3b.png">
