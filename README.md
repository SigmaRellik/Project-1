## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

![NETWORK_DIAGRAM](https://github.com/SigmaRellik/Project-1/blob/438f067bdf48704b395ef192139bff6d24acee9e/Diagrams/diagram.png)

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the **YML** file may be used to install only certain pieces of it, such as Filebeat.

```---
- name: Configure Elk VM with Docker
  hosts: elk
  remote_user: azadmin
  become: true
  tasks:
    # Use apt module
    - name: Install docker.io
      apt:
        update_cache: yes
        name: docker.io
        state: present

      # Use apt module
    - name: Install pip3
      apt:
        force_apt_get: yes
        name: python3-pip
        state: present

      # Use pip module
    - name: Install Docker python module
      pip:
        name: docker
        state: present

      # Use sysctl module
    - name: Use more memory
      sysctl:
        name: vm.max_map_count
        value: "262144"
        state: present
        reload: yes

      # Use docker_container module
    - name: download and launch a docker elk container
      docker_container:
        name: elk
        image: sebp/elk:761
        state: started
        restart_policy: always
        published_ports:
          - 5601:5601
          - 9200:9200
          - 5044:5044

      # Use systemd module
    - name: Enable service docker on boot
      systemd:
        name: docker
        enabled: yes
  ```
  
  
This document contains the following details:
- Description of the Topology
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly **available**, in addition to restricting **inbound access** to the network.
- _TODO: What aspect of security do load balancers protect? What is the advantage of a jump box?_ Availabilty

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the **system and system metrics**.
- _TODO: What does Filebeat watch for?_ collects event logs and detects which files have changed and when
- _TODO: What does Metricbeat record?_ detects changes in system metrics

The configuration details of each machine may be found below.
_Note: Use the [Markdown Table Generator](http://www.tablesgenerator.com/markdown_tables) to add/remove values from the table_.

| Name      | Function   | IP Address | Operating System |
|-----------|------------|------------|------------------|
| Jump Box  | Gateway    | 10.0.0.4   | Linux            |
| DVWA 1    | Webserver  | 10.0.0.5   | Linux            |
| DVWA 2    | Webserver  | 10.0.0.6   | Linux            |
| DVWA 3    | Webserver  | 10.0.0.7   | Linux            |
| ELK       | Monitoring | 10.1.0.4   | Linux            |

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the **Jump-Box** machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:
- Home IP address

Machines within the network can only be accessed by **Jump-Box**.
- _TODO: Which machine did you allow to access your ELK VM? What was its IP address?_ Jump-Box 10.0.0.4

A summary of the access policies in place can be found in the table below.

| Name     | Publicly Accessible | Allowed IP Addresses |
|----------|---------------------|----------------------|
| Jump Box |  Yes                | Home IP address      |
| DVWA 1   |  No                 | 10.0.0.4             |
| DVWA 2   |  No                 | 10.0.0.4             |
| DVWA 3   |  No                 | 10.0.0.4             |
| ELK      |  No                 | 10.1.0.4             |

### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because...
- One can automate the installation for multiple virtual machines

The playbook implements the following tasks:
- Install Docker
- Install python3-pip
- Install Docker module
- Increase virtual memory
- Use more memory
- Download and launch a docker elk container
- Enable service docker on boot

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

![DOCKER](https://github.com/SigmaRellik/Project-1/blob/0da5cf7d2f910f47b46f732e81db68fac1a2a964/Linux/docker_ps.png)

### Target Machines & Beats
This ELK server is configured to monitor the following machines:
- DVWA 1: 10.0.0.5
  DVWA 2: 10.0.0.6
  DVWA 3: 10.0.0.7

We have installed the following Beats on these machines:
- **Filebeat and Metricbeat**

These Beats allow us to collect the following information from each machine:
- **Filebeat: Monitors the log files or locations that you specify, collecting log events including which files have changed and when. It is forwarded to either Elasticsearch or Logstash**

  **Metrictbeat: Periodically collect metrics from the operating system and from services running on the server. The statistics that it collects is forwarded to either Elasticsearch or Logstash.**

![FILEBEAT](https://github.com/SigmaRellik/Project-1/blob/2fadeac884df00fab42cebe16f5e386acbf0379e/Linux/Filebeat.png)
![METRICTBEAT](https://github.com/SigmaRellik/Project-1/blob/2fadeac884df00fab42cebe16f5e386acbf0379e/Linux/Metrictbeat.png)

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- Copy the **playbook file to the Ansible Control Node**.
- Update the **host file to include IP addresses**
- Run the playbook, and navigate to **virtual machines** to check that the installation worked as expected.

_TODO: Answer the following questions to fill in the blanks:_
- _Which file is the playbook? Where do you copy it?_ **YML File and ./etc/ansible**
- _Which file do you update to make Ansible run the playbook on a specific machine? How do I specify which machine to install the ELK server on versus which to install Filebeat on?_ **host file located in ./etc/ansible and to specify you link IP to specific group such as [webservers] or [elk]**
- _Which URL do you navigate to in order to check that the ELK server is running?_ **ELK server public IP**

_As a **Bonus**, provide the specific commands the user will need to run to download the playbook, update the files, etc._
