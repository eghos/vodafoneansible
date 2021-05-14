## Name

Deploying nodejs api using ansible playbook.

## Description

This ansible playbook deploys node js api to existing AWS infrastructure pre-provisioned using CloudFormation. It uses the "api" role. The role has a folder  structure which includes tasks and vars where the playbook and variables are defined respectively. The api.yml file is the playbook file which points to the role so ansible knows it has to deploy the tasks within the role.
Ansible uses ssh configuration to access the EC2 instances, and in this case the pre-provisioned key pair that was used by CloudFormation in provisioning the EC2 instances. The project uses the ec2.py for dynamic inventory, and ec2.ini is the config file for ec2.py, which can be used to limit or control the dynamic inventory but in this case it is left open because I didn't have to get any set of servers for any specific purpose. The configuration file where global variables are defined is ansible.cfg.


## Deplyment

Ansible accesses target nodes using ssh, and the playbooks need to be deployed from a control node, and the control node needs to have pre-requisite packages installed. This guide assumes  you have the control node ready. If not, there are several resources available one of which is the link below:
https://www.tecmint.com/install-and-configure-an-ansible-control-node/

To deploy pull this code to your node, configure ssh either by making it available to the cli by running: `ssh-add <path_to_ssh_key>`, or adding ssh config to the ansible.cfg as shown below:

```
[ssh_connection]
ssh_args = -F <path_to_ssh_key>
```

Note that this can also be defined at inventory group level.

Once ssh config is done, you can then run the following command to deploy the playbook

ansible-playbook api.yml -i ec2.py -u ec2-user

Notice that flag -i meaning inventory points to ec2.py file for dynamic inventory. For amazon linux instances, the username to login is ec2-user. You may replace that with the equivalent if you are not using amazon linux instances. You can add 

he api can then be called at : `curl -X POST http://<instance_ip_address>/app`


## Usage

Once the playbook finishes  running without any errors, it would have installed all required packages on the  EC2 instances, pulled down the application from github code and started the application on port 3000. On making the request: curl -X POST http://<instance_ip_address>/app, id and timestamp would have been added to the dynamo DB table defined in the code. 

## Contribution

This is far from finished. This is only a product of a quick glance at ansible and grabbing the  basic concept to provision this application. Some of the tasks could be replaced by actual modules; perhaps  nginx should have its own role; and other components as monitoring and other checks are not in place. This is merely a proof of concept.

## License 
Vodafone
