## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

Images/ELK_Stack_Project_Diagram.pdf

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the playbook (.yml) files may be used to install only certain pieces of it, such as Filebeat.

Ansible/ansible_config.yml - to install the DVWA servers
Ansible/install-elk.yml - to install the ELK stack on the VMs
Ansible/filebeat-playbook.yml - to install Filebeat on ELK and DVWA servers
Ansible/metricbeat-playbook.yml - to install Metricbeat on ELK and DVWA servers

This document contains the following details:
- Description of the Topology
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly available, in addition to restricting access to the network.
- Load balancers protect the system from DDoS attacks and ensures the system is available.  Availabilty is part of the CIA triad security model for internet technology.
  A jump box allows for one source to administer updates and security tasks to the system securely without needing to take the system dowm. 

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the logs and system traffic.
- Filebeat monitors specific log files, logs events, and sends them for review.
- Metricbeat collects all of the event logs and data and sends the to specific application for analysis.

The configuration details of each machine may be found below.

| Name                 | Function       | IP Address                       | Operating System     |
|----------------------|----------------|----------------------------------|----------------------|
| Jump Box Provisioner | Gateway        | 52.175.202.140 (Public) 10.0.0.7 | Linux (ubuntu 18.04) |
| Web -1               | DVWA Webserver | 20.98.93.233 (Public) 10.0.0.8   | Linux (ubuntu 18.04) |
| Web -2               | DVWA Webserver | 20.98.93.233 (Public) 10.0.0.9   | Linux (ubuntu 18.04) |
| ELK Server           | ELK Stack      | 13.90.88.254 (Public) 10.1.0.4   | Linux (ubuntu 18.04) |
 
### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the Jump Box Provisioner machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:
- Home Network IP Address

Machines within the network can only be accessed by SSH.
- The ELK Server is only accessible via SSH from the Jump Box Provisioner and the Home Network IP address.  The Elk Servers IP address is 10.1.0.4 (13.90.88.254 Public).

A summary of the access policies in place can be found in the table below.

| Name                 | Publicly Accessible        | Allowed IP Addresses                             |
|----------------------|----------------------------|--------------------------------------------------|
| Jump Box Provisioner | No                         | Home Network IP Address                          |
| Web -1               | Only through Load Balancer | 10.0.0.7 (Jump Box) 20.98.93.233 (Load Balancer) |
| Web -2               | Only through Load Balancer | 10.0.0.7 (Jump Box) 20.98.93.233 (Load Balancer) |
| ELK Server           | No                         | 10.0.0.7 (Jump Box) HomeNetwork Port 5601        |

### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because...
- The main advantage to automating confoguration of servers is the ability to quickly deploy multiple servers at one time without having to physically go into each server to set it up.

The playbook implements the following tasks:
- Install docker.io
- Install pip3
- Increase the virtual memory
- Download and configure the ELK container
- Set the published ports
- Enable on boot

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

Images/docker_ps_output.pdf

### Target Machines & Beats
This ELK server is configured to monitor the following machines:
- Web-1 10.0.0.8
- Web-2 10.0.0.9

We have installed the following Beats on these machines:
- Filebeat
- Metricbeat

These Beats allow us to collect the following information from each machine:
- Filebeats collects log files from specific locations such as web logs, packages them up neatly and ships them off for analysis.
- Metricbeats records statistics and analyzes for anomolies.

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- Copy the filebeat-playbook.yml and metricbeat-playbook.yml files to /etc/ansible/files.
- Update the hosts file, which contains the IP addresses of machines you want to configure wiht ansible.  There are separate areas to include webserver and ELK server addresses.
- Run the playbook, and navigate to http://13.90.88.254:5601/app/kibana to check that the installation worked as expected.

_As a **Bonus**, provide the specific commands the user will need to run to download the playbook, update the files, etc._
- ssh azdmin@52.175.202.140         Connect to Jump Box Provisioner
- sudo docker container list -a     Locate ansible container
- sudo docker start stupefied_haslett
- sudo docker attach stupefied_haslett
- cd /etc/ansible/
- ansible-playbook filebeat-playbook.yml
- ansible-playbook metricbeat-playbook.yml
- in browser, navigate to 13.90.88.254:5601/app/kibana

Sources:
https://www.elastic.co/guide/en/beats/metricbeat/7.14/metricbeat-overview.html
https://www.tablesgenerator.com/markdown_tables#
https://www.forcepoint.com/cyber-edu/cia-triad#:~:text=The%20Central%20Intelligence%20Agency&text=CIA%20%2D%20Confidentiality%2C%20Integrity%20and%20Availability,various%20parts%20of%20IT%20security.
https://avinetworks.com/what-is-load-balancing/#:~:text=Load%20Balancing%20plays%20an%20important,to%20a%20public%20cloud%20provider.