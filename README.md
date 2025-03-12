# Ansible Zero to Hero

Day 1: Introduction to Ansible and Getting Started

- Overview of Ansible: What is Ansible, its advantages, and why use it?
- Comparison with Shell and Python scripting for automation.
- Installing Ansible on different platforms.
- IDE(VS Code) and Plugin configuration.

- ![image](https://github.com/user-attachments/assets/5c5dc1b3-9c43-43a5-8ab8-c87218f24ffd)
Ansible is a automation platform  where it can help for provisioning,configuration,deployment(CI/CD) & Network Automation.
in terms of provisioning creating infrastructure,creating ec2 instances,creating platform,interms of configuration it can help for life of devops engineer or sys admin by managing configuration 100's of vm.it can usefull for deployment phase and network automations.

Day 2: Ansible Adhoc Commands

- Passwordless Authentication
- Ansible Inventory 
- Understanding Adhoc commands and their usage.
- Examples of common Adhoc commands for system management tasks.
- Exploring the power of Adhoc commands for quick tasks.

  If a devops want to install ngnix for multiple machine through YML / Adhoc commands in control Node it will instruct that to manage nodes also it will be blocked bcz of authentication so need to know Password or SSH Keys.That is what mentioned in Diagram,,,,![image](https://github.com/user-attachments/assets/6e747a63-cfed-4075-9bb2-25d4888f0b64)

Instead we setup passwordless authentication between ansible control node and manage node it will execute the instruction but we need passwordless authentication should be there so what it is ---It is a mechanisim where u tell the vm for eg a and b. Using the passwordless authenction to B anytime if u getting any instruction VM from A do not ask access for A allow the Access If it tried to coneect give access. For that we have to Initially only for the first we need to provide the password or PEM File from next time it wonts ask password like that.Anyone can connect. So the core ways is the command 
ssh -copy -id there are two ways 1 by password and ssh key for the first time.whatever the organization is used.

Because in some organizaation block passwordelss authentication

Day 3: Writing Your First Ansible Playbook

- Understanding YAML basics and Ansible playbook structure.
- Introduction to Ansible structure: Playbook, Play, Modules, Tasks and Collections.
- Hands-on: Writing a playbook to install apache2 and deploy a static app on aws.

Day 4: Understanding Ansible Roles

- What are Ansible roles and why use them?
- Exploring the folder structure of Ansible roles.
- Comparing roles with playbooks and understanding their advantages.
- Hands-on: Creating a simple role and using it in a playbook.

Day 5: Deep Dive into Ansible Roles with Demo

- Ansible Galaxy - Exploring pre-built Ansible roles.
- Ansible Galaxy - Importing and Installing roles.
- DEMO: Advanced usage of Ansible roles with a practical example project.
- Best practices for organizing roles and playbook structure.

Day 6: Ansible Variables and Precedence

- Create AWS Resources using Ansible (Collections)
- Understanding Ansible variables and their scope with an example
- Jinja2 Templating - Utilizing advanced templating features
- Variable precedence: How Ansible resolves conflicts between different variable sources.
- Hands-on: Using variables in playbooks and roles.

Day 7: Ansible Conditionals and Loops

- Using conditionals in Ansible to control task execution.
- Implementing loops for repetitive tasks.
- Practical examples of conditionals and loops in playbooks.

Day 8: Error Handling in Ansible

- Dealing with errors and failures in Ansible playbooks.
- Error handling techniques and best practices.
- Demonstrating error handling in practical scenarios.

Day 9: Ansible Vault for Security

- Understanding Ansible Vault and its role in securing sensitive data.
- Encrypting and decrypting files using Ansible Vault.
- Best practices for managing secrets and sensitive data in Ansible.

Day 10: Policy as Code

Day 11: Network Automation using Ansible

Day 12: Ansible Tower Deep Dive

- Understanding Ansible Tower
- Comparision with Ansible command line and adhoc commands
- RBAC and Security with Ansible Tower

Day 13: Advanced Ansible Project

- Terraform + Ansible Project

Day 14: Ansible Interview Questions
