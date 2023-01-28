Role: aws-tag
=================

Ansible role to create,modify and remove tags for any EC2,ELB,VPC-SUBNETS,TG resource.

Requirements
------------
1. Ansible installed on the base machine.
2. AWS Cli Configured locally

Role Variables
--------------
Tags `Location` & `Environment` are global Variables and will be applied to each resources. Where as `Name` tag is resource specific.
```yaml
tags:                                                #Common Tags for all the resources
  Location: us-west-2
  Environment: production
```

Add the desired region, state, instance-id, and Value of tags, of the instance you want to tag, as the list of items in file `vars/main.yml` as shown below.
```yaml
region: us-west-2                                    #The AWS region to use.
state: present                                       #present(default) to create , absent to remove tags
ec2_tags: :                                                #key: value  
  Name: foo-bar                                      #Change Value of Name to your need
instance_id:                                         #The EC2 resource id list.
    - i-xxxxxxxxxxxxxxxxx
    - i-xxxxxxxxxxxxxxxxx
```
Add the desired name of the VPC you want to tag, as the list of items in file `vars/main.yml` as shown below.
```yaml
region: us-west-2                                    #The AWS region to use.
state: present                                       #present(default) to create , absent to remove tags
vpc_tags:                                                #key: value  
  Name: foo-bar                                      #Change Value of Name to your need
vpc_name:                                            #The VPC resource name.
    - vpc-xxxxxxxxxxxxxxxxx
    - vpc-xxxxxxxxxxxxxxxxx
```

Add the desired region, loadbalancer you want to tag, as the list of items in file `vars/main.yml` as shown below.
```yaml
region: us-east-1                                    #The AWS region to use.

# Define CLB names, tags
classic_elb_name:                                    #The CLB resources name.
  - myclb
  - myclb
classic_elb_tags:
    Name: foo-clb                         #Change Value of Name to your need     (In case of removing tag, leave the value empty)

# Define ALB names, tags
application_elb_name:                                #The ALB resources name.
  - myalb
  - myalb
application_elb_tags:
    Name: foo-alb

# Define NLB names, tags
network_elb_name:                                     #The NLB resources name.
  - myalb
  - myalb
network_elb_tags:
    Name: foo-nlb
```
Add the desired region, Target Group you want to tag, as the list of items in file `vars/main.yml` as shown below.
```yaml
region: us-east-1
tg_name:                                             #Target Groups Name
  - foo-tg1
  - foo-tg2
tg_tags:                                             #Target Groups Tags
    Name: foo-tg 
```
Add the desired region, Elastic IP's pubic address you want to tag, as the list of items in file `vars/main.yml` as shown below.
```yaml
region: us-east-1
public_ip:                                           #Public IP of the EIP
  - 44.xx.xx.xx
  - 44.xx.xx.xx
eip_tags:                                            #Tags added to the EIP of the Public IP
    Name: foo-eip
```
Add the desired region, Security Group ID you want to tag, as the list of items in file `vars/main.yml` as shown below.
```yaml
purge_tags: false                                     #If purge_tags=true and tags is set, existing tags will be purged
group_id:                                             #Security Group IDs
 - sg-xxxxxxxxxxxxxxxxx
 - sg-xxxxxxxxxxxxxxxxx
sg_tags:                                              #Security Group Tags
    Name: foo-sg
```
Add the desired region, db_identifier details you want to tag, as the list of items in file `vars/main.yml` as shown below.
```yaml
purge_tags: false                                     #If purge_tags=true and tags is set, existing tags will be purged
db_instance_identifier:                               #db identifier of AWS RDS
 - database-1
 - database-2
rds_tags:                                             #RDS tags
     Name: foo-rds
```

Example Playbook
----------------

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

    - hosts: localhost
      roles:
         - { role: aws-tag }

NOTE
----
1. This Module will create tags `Name`, `Location`, `Environment` for the AWS Resources if not **present**.. 
2. If there is any **existing tags** `Name`, `Location`, `Environment` are **present already** on the resources, it will **override** the tags with the value defined in the variable of this module.
3. **Ansible tags** are used to call the desired module to tag your AWS Resources

Run the playbook to create the tags. (sample playbook create-tag.yaml)
```shell
ansible-playbook create-tag.yaml --tags=ec2                  #To tag EC2 Resources Only
ansible-playbook create-tag.yaml --tags=vpc                  #To tag VPC Resources Only
ansible-playbook create-tag.yaml --tags=public-subnet        #To tag Public Subnet Resources Only
ansible-playbook create-tag.yaml --tags=private-subnet       #To tag Private Subnet Resources Only
ansible-playbook create-tag.yaml --tags=application-lb       #To tag Application Load Balancer Resources Only
ansible-playbook create-tag.yaml --tags=classic-lb           #To tag Classic Load Balancer Resources Only
ansible-playbook create-tag.yaml --tags=network-lb           #To tag Network Load Balancer Resources Only
ansible-playbook create-tag.yaml --tags=targetgroup          #To tag Target Group Resources Only
ansible-playbook create-tag.yaml --tags=elastic_ip           #To tag Elastic IP Resources Only
ansible-playbook create-tag.yaml --tags=security-group       #To tag Security Group Resources Only
ansible-playbook create-tag.yaml --tags=aws-rds              #To tag AWS RDS Resources Only
```
To run the Whole playbook
```shell
ansible-playbook create-tag.yaml
```
To run the playbook with selected tags
```shell
ansible-playbook create-tag.yaml --tags=vpc,public-subnet,private-subnet
```