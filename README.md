

Role Name
=========
<h2>localhost</h2>
<strong>VPC</strong> #Creation of a virtual private cloud <br>
<strong>subnets</strong> # Creation of subnets on different availability zones<br>
<strong>IGW</strong> # Creation of internet gateway for our ec2 instances to have internet access<br>
<strong>route_tables</strong> # Creation of route tables for subnets to have proper routing to the IGW<br>
<strong>security_groups</strong> # Creation of firewalls for our subnets<br>
<strong>ec2_instances</strong> # Creation of instances(ec2 and rds),adding the ec2 instances to the hosts and creating a variable file for the rds instance endpoint<br>
<strong>load_balancer</strong> # Creation of a load balancer and attaching the ec2 instances to it.<br>
<br>
<h2>wpservers</h2> #These are the ec2 instances<br>
<strong>server</strong> # Installation of required softwares on our hosts <br>
<strong>mysql</strong> # Creation of a mysql database in the rds instance <br>
<strong>php</strong> # Installation of php dependencies <br>
<strong>wordpress</strong> # Installation of wordpress and modifying its config file to connect to our database<br>

Requirements
------------

Virtual machine that runs on Ubuntu or any other linux-based OS with ansible installed on it, preferably latest version. Change the access keys in the variables as well to run, then, supply your own .pem and configure it in the ansible.cfg file. Don't forget to change the permission of .pem file to read only (chmod 400)

Example of editing in ansible.cfg (etc/ansible/ansible.cfg):<br>
[defaults]
host_key_checking = False <br>
private_key_file = /home/ansible-key.pem


Role Variables
--------------

I have two variable files, one in the VPC role, and the other in the server role. The variable file in the VPC role provides to all the localhost play, the variable in the server role provides to the 'wpserver'(the ec2 instances) host play.

Example Playbook
----------------
   #CREATE DATABASE<br>
 - name: Create an RDS MySQL DATABASE<br>
   rds:<br>
    command: create<br>
    wait: yes<br>
    wait_timeout: 1500<br>
    aws_access_key: "{{ aws_access_key }}"<br>
    aws_secret_key: "{{aws_secret_key }}"<br>
    region: "{{ region }}"<br>
    instance_name: "{{ db_name }}"<br>
    db_engine: MySQL<br>
    size: 5<br>
    publicly_accessible: yes<br>
    instance_type: db.t2.micro<br>
    username: "{{ db_user }}"<br>
    password: "{{ db_pw }}"<br>
    vpc_security_groups: "{{ sg_of_rds.group_id }}"<br>
    subnet: "{{ vpc_name }}"<br>
   register: rds<br>
          
Author Information
------------------

Michihiro Yamamoto - Technical Consultant at DXC 
myamamoto4@dxc.com
