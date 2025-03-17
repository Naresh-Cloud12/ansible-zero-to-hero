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
roles so to explain roles in anable let me put you you some questions so in the previous example that we have seen there are only two task in the answerable Playbook because we were writing our very first answerable Playbook so we have chosen a very simple answerable Playbook that has only two tasks but imagine a scenario where you have an anible Playbook that has 50 number of tasks or an anible Playbook with 60 number of tasks not just the number of tasks but imagine a scenario where within your anible playbook you have post then you have multiple number of variables so let's say the case where you have 20 variables and then you have tasks where within the task section you might want to put some 40 50 tasks then you have something called handlers I'll explain what is handlers don't worry then you have the meta information of the anable Playbook so what happens in this case is not just that the size of your anible playbook goes high but this anible Playbook that you are writing will also lose things like readability right whoever is reading this anable Playbook imagine you open a Playbook yaml file right main. yaml file or you know your first Playbook do yaml file or whatever is the name of the Playbook you open a file and imagine you see 2,000 lines the moment you see this Playbook yaml file with 2,000 lines that is a red flag and will lose interest in reading the file but instead imagine the case where you can split this Playbook okay you can clearly see here there are multiple sections there is a section for variables there is a section for tasks there is a section for handlers there is a section for metadata what if you split the sections and each section you put in a different folder and within the folder you have individual yaml files you know at the first site you might feel it complicated you might think abishek I can write this entire playbook in one yaml file instead why should I create a folder for vs then why should I create a folder for tasks why should I create a folder for handlers why should I create a folder for meta in instead I can write only one yaml file but you know when you see a yaml file which is as simple as this everything looks good great but when you see the same yaml file with 2,000 lines immediately you will say that anible is complicated and I don't want to write the code in anible you know so that brings to the concept called as readability and modularity not just for anible for any programming language it can be python it can be go it can be even languages like terraform readability and modularity is very very important and to address that readability and modularity anible brings a concept called as roles where in simple language without any complication if you want to understand what is a role role is nothing but take a anible Playbook and if you split the different sections in the anible Playbook into different folders and place them under one umbrella called as role then that becomes Your anible Role so you don't ave to learn anything additional in terms of anible role all that you need to do is if you know how to write a anible Playbook yaml file you can just take the individual parts and put them under each and every folder now you might say but abishek do I need to create this folders each and every time is it not complicated don't worry what anible does is let me pull my terminal so anible can create that entire folder structure for you by just running one command and the command is an Galaxy so we are learning a new command at this point of time till now we learned two anible commands one is the anible command itself when you want to execute ad hoc commands next we learned anible Playbook command when you want to execute the anible playbooks now I am explaining a new command which is called as anible Galaxy and this command is going to play a very very crucial role because this command will simplify creating managing deleting and listing of the anible roles now let me quickly show you how to create a role using anible Galaxy it's very very simple all  that you need to do is anible Galaxy followed by role followed by in it right anible galaxy role in it and provide the name of the role that you want to create let's say the name of the role that I want to create is test so it said role test was created successfully

you keep telling complicated anible playbooks what exactly are they can you give some examples for sure let's take example of installing and configuring kubernetes you know not every organization go for managed kubernetes services such as e or AKs you know in your organization you might have a requirement to install kubernetes on the control nodes and the worker nodes terraform can create the infrastructure terraform is used to create the infrastructure right it's a ISC tool where you might spin up the ec2 instances VPC configuration but when you want to install the packages on the ec2 instances or the farget instances that is where anible comes into picture and this can be a complicated role because to install kubernetes you have your control nodes then you have your worker noes and on the control nodes you need to install and configure different workloads API server etcd then you have to set up uh container runtime container Network right and again on the worker nodes you might want to set up the cubet you might want to set up how worker nodes are joining with the uh control nodes so overall this Playbook like I've explained my easily go up to 50 60 tasks right so imagine you joined an organization and the seniors in the organization have given you a Playbook doyl file which has kubernetes installation and configuration you will see a file like this and immediately you will say it's very difficult to understand instead the same example kubernetes or another example let's say installation and configuration of databases like orle again there will be so many steps to install a version of orle configure that version of orle or any database so such things by using roles you can make your playbook more readable more modular and in future in future using the anible Galaxy you know anible provides something called as anible Galaxy the one that we saw here is the command line utility for the same right so using this anable galaxy you can create roles and you can upload that roles into this anible galaxy or to your GitHub and you can share the roles within your organization so the other advantage that roles brings here along with the routability and modularity this can be also shared across your or organization because roles can be uploaded to you know you can assume anable Galaxy as a Marketplace just like Docker has Docker Hub right Amazon has a Marketplace 

the other advantage that roles bring is readability modularity and also sharing across team's organization and across everyone outside your organization 
![image](https://github.com/user-attachments/assets/c7267b49-9874-4a6d-af52-0868907d5090)
![image](https://github.com/user-attachments/assets/fbc87633-7c50-4982-bd77-10773a7b2848)


How to convert ansible playook to role use the galaxy command ![image](https://github.com/user-attachments/assets/66a3a280-ba53-48bd-a4a4-28dd11680382)

I explained this In First Class where if something is already available the thing that you are trying to do if it is already available on the target machine or the manage nodes anible will not execute that again if you're taking shell script or python for an example what happens with shell script if you're telling shell script to create a directory even though the directory is present okay for example if I do mkd ABC okay now ABC folder is available if I again do mkd ABC shell script will fail or the shell command will fail this nature is not called as itm poent whereas if you do the same thing with anel create a folder once and create the folder again anible will not create the folder because it says the folder is already available I don't have to create it this nature is called as item poent which is very important characteristic of anible that's why here you can see that the number of tasks are executed three but nothing is changed on the target machine so now let's connect to one of the ec2 instance and let's delete that HTM file let's see if an is going to create it or not that way we can confirm our roles are working right previously the configuration was already there so it did not create anything so let's take manage node one and uh connect let's use the E to instance connect I'll take the pseudo uh user and 

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
