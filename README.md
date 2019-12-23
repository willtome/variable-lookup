# Variable Mapping
This repository provides 2 examples for mapping variables for an IPAM check use case. The fist example, all-in-one, is implemented in a single playbook. The second example is designed to be used in an Ansible Tower workflow.

In this use case, IP information provided via inventory variables needs to be checked. The variable names that need to be checked will vary depending on which playbook is being run. 

## All-in-one
`all-in-one.yml` shows an implementation where a role is included prior to the other operations/changes being made. This requires altering existing playbooks. This method does allow you to use filters (as shown) to manipulate variables and capture your variable mapping in source control. Run this example using the following command.
```
ansible-playbook.yml -i inventory all-in-one.yml
```
## Workflow
This alternate solution uses a Tower workflow to prevent having to alter each playbook. This does require a workflow to be created for each playbook you want to IP check and mapping information is entered into Ansible Tower. To configure in Tower...
1) Create a Project using this git repository.
2) Create an Inventory with a dynamic source using the inventory file in this repo. 
3) Create a Job Template for the `ipam_check.yml` playbook. Select "prompt on launch" extra variables. 
4) Create a Job Template for the `playbook.yml` playbook. Add a survey to prompt for the variable `HOSTS`.
5) Create a workflow. Add `HOSTS: routers` to the `extra_vars` section of the Workflow. 
6) In the Workflow Visualizer, add the ipam_check Job Template. When selecting it, click Prompt and enter:
   ```
   IP:
     - tunnel_ip
     - ntp_servers
   ```
7) Add the the playbook Job Template to the Workflow visualizer. 
8) Run the workflow.