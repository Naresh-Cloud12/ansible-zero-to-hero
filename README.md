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

Day 5: Deep Dive into Ansible Roles with Demo(( Watch the 20 minutes from start u can understand how github creation nd rest)))

- Ansible Galaxy - Exploring pre-built Ansible roles.
- Ansible Galaxy - Importing and Installing roles.
- DEMO: Advanced usage of Ansible roles with a practical example project.
- Best practices for organizing roles and playbook structure.

   I'm also going to explain how to publish your anible roles onto the anible Galaxy this is very very important
03:03

because as a devop enger you write some anable playbooks and you might want to share it with other people in your organization outside your organization so that is done again using anible Galaxy we will learn that in this video we also have a demo where I'm going to show you if you are given a task to write a complicated configuration management task using anible how you can effectively use anible Galaxy to use some existing roles in the anable Galaxy server and how you can make your life as a devops engineer
03:42

simple you almost don't have to write code most of the times because most of the anible related modules task roles are already published to anible Galaxy so please watch this video till the end it's going to be very interesting so in the last class we had a glimpse of this website galaxy. an.
04:07

com where I've explained that anible Galaxy can be understood as a Marketplace for anible roles what does that mean in anible Galaxy you have thousands of ansible roles if you just go to the rool section you can see thousands of anible roles that are written and published by different devops in iners and system administrators for example if I am given a task let's say that I'm given a task to write an anable playbook for installation and configuration of Docker at the first site this might look very simple to you installing and
04:52

configuring Docker okay it's very easy but what if your anible control node talks to different manage nodes and you have to install Docker on all of these manage nodes where one of the manage node is using Red Hat Linux one of the manage node is maybe using Debian one of the manage node is using ubu one of the manage node is using Alpine right similarly one might be using windows so in each of these virtual machines the command that you use to install Docker changes the command that you use to create a Docker
05:41

user changes the command that you use to restart your Docker service might change so the complexity of the anable Playbook will go high when you have different kinds of configuration that's the whole purpose of anible right the configuration management where you you have different kinds of virtual machines even the simple things such as installation of Docker the complexity can be high because you have to handle different uh conditions in your anable playbook if the virtual machine is Red Hat then perform 1 2 3 four five tasks you know
06:20

if the virtual machine is Debian then perform different kind of tasks if the virtual machine is uban to you need to change the task so the number of tasks in your playbooks go High and the conditionals that you use also goes high so this is where instead of writing anable Playbook or anible role what you can do because Docker is a pretty common task first thing that you have to do is go to anible Galaxy and search and see if there is some role that suits your requirement I'll just search for Docker and I can see that okay there is a
06:58

anable role Docker for Linux and if I click on it I can read the documentation okay it says the role is tested on uban to Debian red hat so it is tested on multiple which suits most of my requirement and I can also look into the GitHub repository which hosts the source code for this role so I can also look at the GitHub repository and see the standard of the code that is written I can go to the task folder and I can see you know if I can actually trust this author or not so this is how anable roles can help you instead of writing common task if
07:44

you are lucky you might also find some tasks that are very unique to your organization but someone somewhere might have faced the similar requirement and you have a similar role in the anible Galaxy so like I explained in the last class this is the major advantage of anable roles it will bring modularity to your code right it will bring readability to your code and along with that you can also share the anible roles within your organization and outside your organization using anible Galaxy now saying all this abishek how
08:26

do I actually use the roles from this right if it is Docker Hub which is very similar to anible Galaxy right as a devop iner if you already know what is dockerhub it's similar to anible Galaxy where it's a Marketplace for Docker images you have thousands of Docker images and you can use the images using the docker CLI right I can use the docker CLI commands like Docker build Docker push Docker pull similarly for anible you you have something called as anible Galaxy just like for Docker Hub you have the docker C for anible Galaxy you have
09:09

this CLI utility called anible Galaxy most likely you will get the anible Galaxy installed along with your anible itself you can just search for anible Galaxy minus H you will see that you know it says it can be used for two major activities one is for collections and one is for rules exactly what you see here as well you know you have collections and roles at this point of time we are not discussing about collections our focus is roles so you can simply say anible Galaxy Ro minus H so what does it say it says that one
09:57

is it can do in it initial ization of a role which we saw in the last class where we created a role called httpd using anible Galaxy in it it created the complete folder structure for us with all the required basic files then it says remove just like in it initialize a anible uh role for you using remove you can remove any roles that are available in your role path then you can do delete list search import install and setup we will go step by step and we will try to understand how to use anible Galaxy for all our
10:41

activities the very first thing that I'll do is okay abishek let's talk about the activity that you have mentioned one is how can I use the anible rules that are available in anable Galaxy let's take the same example and can you show us how can we use this Docker anible role that is in the Galaxy so very simple what you need to do okay the very first thing that you do is set up an account with anible Galaxy very simple to your top right side you have the login button once you click on the login button you will see a page which asks
11:25

you to authenticate you can use your GitHub account so when you use your iub account you will be signed in with your GitHub user which is what I have done I'm signed in with my GitHub user and once you signed in just like others in the anible Galaxy you also get your anible role name space which is just like your anible Galaxy account or your user space at this point of time I haven't published any roles here so it says roles will appear once imported you know once I publish any roles to the anable Galaxy it will be shown here at
12:05

this point of time I haven't published any if you look at the previous user from which I was using the docker anible role if you go to this user so you can see in this users's role name space there are multiple anible roles because this user has published and this particular role is downloaded by 40 three people this particular role is downloaded by 62 this is 248 and this is 1783 so it's almost like a community work where you are sharing your anible role with others perfect now our topic of interest
12:45

is Docker so first let's see how to install this role so I don't have anything I just have my previous anable playbooks and rules which we have written till day four now I'll implement the task where I will install and configure Docker on two of my manage notes which I have set up with passwordless authentication in day two so for that first thing that you do is go back to your anel Galaxy just copy this command here what does it say anible Galaxy rooll install this particular role and as soon as I do that it says
13:30

role installed you know basically I already have this role while I was doing the demo I have installed it so it says already installed now where does this role go so if you just do LS tilt a hidden folder called anible roles so you can see all the roles that you have installed so it says I have installed these three roles so this is the one that you should see when you run the command and where will you see in the home directory do anible folder which is a hidden folder and inside that you have roles perfect now I have this particular
14:17

thing which says the docker role is installed but abishek how do I use that role in the last class when we wrote our very first anible role you said in the first Playbook do yaml that is in the Playbook you just mention the reference and say roles as httpd and it understood that httpd is available in the same folder so it started using it there is no difference you can do the same thing where what I'll do is I'll just copy the same Playbook okay copy or let me write it so whm docker Playbook do yaml so I'll start with
15:04

three hyphens then I'll just say host all then I'll say become true or remote user root you can use anything then I'll say roles what was the name of the role we need to check that again just copy this one and and paste it here okay now let's save this file and run the command anible Playbook minus I inventory file followed by Docker Playbook do yaml and that's it you will see all the tasks that are involved if you go to the task. yaml or the main.
16:00

yaml you can see that it has so much code like almost 20 plus tasks here and you have additional yl files where you have setup dean. yaml which is specific like the task specific to Debian then you have task specific to Red Hat path specific to Docker compose so instead of writing this complicated anable Playbook what did I do is I just used the existing anible role in Galaxy and I have installed and configured Docker on my ec2 instances now you can simply log into this e to instance just for your verification and you can see that Docker
16:42

should be installed please try to do this activity uh you know it helps you a lot of times as a devops engineer writing and sible roles will take a lot of time for example kubernetes or database installation configuration you can use the Galaxy and reduce significant amount of time so let's do Docker and see perfect so Docker is installed and Docker user is created all the required things are set up using anible Galaxy if you want to try it out you can create Red Hat you can create Alpine and give it a try you will see that anable Galaxy
17:25

works because the tasks are written for different possibilities it is written for Debian it is written for red hat and also ubo perfect now okay abishek I understood half of it where I can use anible roles in the Galaxy but what if as a devops engineer I want to publish roles to this anible galaxy so in the last class we wrote this anible rule right we wrote the httpd now how do I publish this so very simple thing one thing that you have to understand is anible rules should always be put in GitHub and should be imported into the anable
18:11

Galaxy right same thing we did here also when we are using this anible rule for Docker the source code is pushed to GitHub pushed to a GitHub repository and this particular user has imported this GitHub repository we have to do exactly same thing so if you have a existing anable role first go to that folder httpd and what you need to do is in your GitHub create a new repository okay go to the home space and first create a new repository don't worry all these steps are also mentioned in the anible Z to Hero GitHub repository I have provided
18:58

the notes you can go to the repository and you can read through it and you can also execute or watch this video and execute so let's say dumy Rule and let's provide a description a dummy anible role for demo okay it's a public repository let's keep it and create the repository so I've created this repository now what I'll do is with within this folder okay I think I already have let me delete dogit because I have done the demo previously so what you need to do is first do the git in it okay I have initialized an empty G
19:47

repository now you can just copy the https URL you also have the commands here you can use the commands end of the day what I'm doing is I'm just pushing all the source code here to the GitHub repository that's it you have different ways to do it but I'm trying to explain the simple way now you will do the command get remote origin add this sorry you will do get remote add origin followed by this my bad okay now if you do get remote minus V perfect this is added now all that you need to do is just add stage the files now just
20:39

say get commit minus M initial files whatever you wouldd like to call right and now just push this get push origin main perfect now you should once you refresh you should see that your anible role is pushed onto the GitHub repository and here you will see all the dummy information and where is this getting populated from you can update this readme file so that you can write the appropriate things now I'm not spending time updating this but one thing that you should definitely do is update this meta folder and within the
21:26

meta folder update the main. jaml where you provide all the details like who is the author what is the description what is the company these details have to be definitely mentioned otherwise within your organization people will not understand why is this role written who has written it all this information in the meta yaml file is very very important to write now the only thing that is left is okay abishek the role is also publish to GitHub repository now how do I push it to anible Galaxy so for that you have
22:03

the command anible Galaxy import your GitHub username followed by the name of the repository dami roll right so it will throw the authentication error okay so you have done everything but still it is through throwing authentication error because the first thing that you have to do is you need to go to the anable Galaxy and if you go to the collection collection section you will find API token just load the token copy it and pass it to your anible Galaxy hyphen hyphen token and now you will see that your anible role will be published to Galaxy
22:57

and anybody can start using it there are ways to secure your token as well don't worry I will be showing it you can uh secure by adding it to a file you can also add it to your anable configuration but for now let's see now if I go to my roles if I go to my role name space abishek vamala see there is a role that we have published few seconds ago abishek has published dumy rooll now anybody can look into this documentation which I have not not updated as I mentioned you need to come here and update the readme file it's
23:35

very important like you have seen the previous rule that we have used for Docker installation this file was very very neatly updated see if you go here you can see that the main. yaml so the readme.md is updated accurately that's why it's reflected on the anable Galaxy as well but because I'm doing a demo I have not written that entire thing in your organization you have to write it very clearly and the installation step is anybody who wants to use it they can run this command and just like how we use
24:11

the docker role they can use this anible role which is a dummy role that we have written so this is how you write your own anible role and publish it or use existing anible roles from the anible Galaxy task like Docker and installation kubernetes installation E2 installation most of it will already be available in the anible Galaxy so this is the video for today I hope you have enjoyed it next videos are going to be even more interesting so please try to watch see you all in the next one take care bye-bye


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
