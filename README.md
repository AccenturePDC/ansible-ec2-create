## Overview

This playbook uses ec2 modules to create AWS EC2 instances, security group, and attach an extra EBS volume. 

## General Usage

  - Authenticate ansible-playbook to your AWS account
    
	```bash
	export EC2_ACCESS_KEY=Your AWS Account Access Key
	export EC2_SECRET_KEY=Your AWS Account Secret Key
	```
	
  - To provision EC2 instance, run the command:
  
    ```bash
    ansible-playbook provision.yml --extra-vars "instance_name=MY_EC2_INSTANCE aws_region=${AWS_REGION} key_name=${AWS_KEY_PAIR} vpc_subnet_id=${SUBNET} ami_id=${CENTOS_AMI_ID} instance_type=${MASTERS_INSTANCE_TYPE} volume_size=${ROOT_VOLUME_SIZE} lv_volume_size=${LV_VOLUME_SIZE} vpc_id=${AWS_VPC_ID} volume_device_name='/dev/sda1' env=sandbox vpc_cidr_block=${AWS_VPC_CIDR}"
    ```

The playbook will generate a file called instance_ids.txt which contains the instance id of the provisioned EC2. This is intended for the below playbook to be accepted as a parameter and delete the said instance.

  - To delete the instance you just provisioned. Run the following:

    ```bash
    ansible-playbook terminate_instances.yml
    ```

	
## Environment File

The environment specific variables are passed as parameter to the Ansible scripts using a property file called `all.yml`.

ec2-create Variables | Description
-------------------- | --------------------
ansible_user | User that will execute the ansible scripts. For Rhel and Amazon linux OS,ec2-user is the default username
aws_region | AWS EC2 region is the location of EC2 instances to be launched
key_name | AWS Key pair name
security_group | AWS Security group name which controls traffic to reach an EC2 instance
instance_type | Hardware of the EC2 instance (No. of CPU, RAM, and storage)
ami_id | Image ID of OS type of EC2 instance
volume_device_name | Volume device name
volume_type | Volume type (Magnetic, GP2, IO1)
volume_size | Volume Size in GiB
vpc_subnet_id | VPC Subnet ID
assign_public_ip | Enable/Disable auto-assigning of Public IP to an EC2 instance
allocate_eip | Enable/Disable auto-assigning of Elastic IP to an EC2 instance


