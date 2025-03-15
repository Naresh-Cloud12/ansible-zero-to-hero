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

Lets create 2 Ec2 Instance and different type of approach one is ssh and password

Step 1 - ssh way--

ssh-copy-id -f "-o IdentityFile <PATH TO PEM FILE>" ubuntu@<INSTANCE-PUBLIC-IP> command for terminal and fill the pem files and instance ip address and click yes and exectute the command whatever it mentioned  so Now connected after that use this command SSH -ubuntu @IPAddress now it will connected to from our laptop to the EC2 Instances... SO thi ssh key

Step 2 -- Password way

Go to the 2nd instances created and connect to ssh -i <Path of the keypair> Ubantu@IPAddreess then yes now enter sudo vim /etc/ssh/sshd_config.d/60-cloudimg-settings.conf just say Yes in password authentication.. so in any organization if they are not using EC2 instance so we can give command 
sudo vim  /etc/ssh/ and in that u can able to sshd_Config then enter with  sudo vim /etc/ssh/sshd_config in this file u will field called password authectication commented so go and uncommnet that then we need to restart that using sudo systemctl restart ssh so we need to give password for cnnect using sudp passwd ubuntu enter password now log out
Then ssh-copy-id Ubuntu @IPAddress then we need to enter the password and again ssh ubuntu @IPADrress now connected 

With these we have benifis to runn any script using YAML OR Adhoc commands of having ansible we can able to install or whatever for manage nodes also

How ansible know these will be my control nodes so there is a way that's called ansible Inventory actually its  a file we need to provide a filed we need to puth the User and the ipaddress.

INvertory is heart of the Ansible---Because it will tell ur ansible control node to which servers it has to talk that is what are ur ansible manage nodes.What is the externsion of invertory it can either write in Yaml format or .Ini extension or invertory.ini whatever the extensionshould .ini(dot.ini)
If you dont want to provide invertory file everytime go to the file called /etc/ansible/hosts if its not there create and store the server if you dont provide any ini file that will take automaticallly....So what is the recommended approach YAML?INI 

eg. vim Invertory.ini
--- ansible -i inventory.ini -m ping all ---so all- the server in the invertory files
--- ansible i inventory.ini -m shell -a " apt install openjdk" all
---  ansible -i inventory.ini -m ping ubuntu@IPAddress ---> If u want to 1 server or particular u can mention the server with ubuntu

GROUPING THE SERVERS____
___
So if we have multiple server like 5 db and 4 app server can we grp that server for db and app Yes... for that ![image](https://github.com/user-attachments/assets/446a9045-f29f-497f-8da8-61b4b59c52fc)
now asusual same command  ansible -i inventory.ini -m ping DB istead of all we need to mention what grp we need to run for eg; DB
So for simple Code or simple task like installing apache server,createing files on targetting server,restarting servers,or installing node server we can use adhoc commands....FOr long or complext for eg; terraform,CFT Advantage is resuable ,sharable,Github,collabrable

Day 3: Writing Your First Ansible Playbook

- Understanding YAML basics and Ansible playbook structure.
- Introduction to Ansible structure: Playbook, Play, Modules, Tasks and Collections.
- Hands-on: Writing a playbook to install apache2 and deploy a static app on aws.

  is yaml so what exactly is yaml the textbook definition says yaml is a human readable data serialization language what does that mean let's say you want to write a python application and this python application needs an input which can be as simple as list of users or list of students in a class so basically you want to pass some data as an input to your python application obviously this data can be fed to the python application in different ways one is you can pass this data as input using a text file you can pass this data as input using Json
  what's the problem with text file you know if I'm passing this information through a text file or data through a text file I don't need to learn any other languages such as Json or yaml so text file is not recommended especially if the data is simple it's still fine but imagine if the data is complex and has thousands of line text file is not recommended because it's not a proper format or there is no standard structure where data has to be put in a text file example maybe if I'm writing this text file I can put list of students name where each student name in a new line so someone else who is writing this text file they might put two students name in a single line or someone else who is writing this text file they might put first student name in the first line and maybe give some spaces and second student name in the third line third student name maybe in the Fifth Line so the problem if you are asking user to put the data in a text file and pass it to your application the data can be presented in different formats why because there is no standardization or a set requirement that text file has to be written in this format only otherwise your python application can throw a syntax error that's not possible because there is no syntax or semantics to a text file this is where templating languages such as Json or yaml comes into the picture so they are standard templating language files that are agreed accepted and defined according to the standards so if you don't write or if you don't put data into the Json file as per the requirement of the Json file then you will get syntax errors that means everyone who is writing a Json file or everyone who is writing a yaml file they need to follow a specific standard that's why you will see that in devops when you are passing some information either to kubernetes or to anible and many other applications you use standard templating language such as yaml where data is structured and available in a serialized way you might 

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
