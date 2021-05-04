# Infrastructure as Code (IaC) - Ansible

### Summary:

- Types of Infrastructure:
	- Mutable or Immutable
	- On premises or Cloud

- Infrastructure as Code:
	- What is it?
	- How it works?
	- Two parts of IaC:
		- Configuration Management
		- Orchestration

### CTO's choices on IT infrastructure

- On premises + Immutability
- On premises + Mutability - a lot of company running in this way
- Cloud + Mutability
- Cloud + Immutability - becoming popular

What is mutable or immutable:

Mutable -> can be changed. Immutatble -> can not be changed. 

Largely mostly have used mutable environments, ssh in, run bash, restart servers and get it to the place that the machine needs to be. Making something from scratch has its drawbacks as it might take longer but in the long run it's more efficient as you get more predictable result.

### What is IaC?

- IaC is a method of provision and manage IT infrastructure through the use of machine-readable definition files (i.e. source code like yaml), rather than through old fashioned operating procedures and manual processes.

- Infrastructure as code (IaC) is the process of managing and provisioning computer data centers or cloud resources through machine-readable definition files (source code), rather than physical hardware configuration or interactive configuration tools.

- Infrastructure as Code (IaC) automates the provisioning of infrastructure, enabling your organization to develop, deploy, and scale cloud applications with greater speed, less risk, and reduced cost.

- Infrastructure as Code (IaC) uses a high-level descriptive coding language to automate the provisioning of IT infrastructure. This automation eliminates the need for developers to manually provision and manage servers, operating systems, database connections, storage, and other infrastructure elements every time they want to develop, test, or deploy a software application.

### Benefits

- Speed and simplicity
- Configuration consistency
- Minimisation of risk
- Increased efficiency in software development
- Cost savings

With speed and simplicity we save time and that means saving cost.

How do you increase efficiency in your software development life cycle? How do you save cost? How do you speed up the process?

Using IaC. Is going to make infrastructure in seconds. It will automate everything that we usually do manually. It will make it more easy to implement, our lives will be more easy.

### Best Practices

- Codify everything
- Document as little as possible
- Maintain version control
- Continuously test, integrate and deploy
- Make your infrastructure code modular
- Make your infrastructure immutable (when possible, nobody can go in and change it without your permission)

Example of practices? -> talking to multiple servers from ansible controller and doing differents tasks in the differents servers at the same time rather tan log in to the server and doing that manually.

### Two parts of IaC

__Configuration Management__

Tools are resposible for provisioning (anything that we want) and maintaining the state of your systems. There are a lot of tools available to do this but some of the best known are:

	- Chef
	- Puppet
	- Ansible (go to the app and install everything, the same with db)

We are going to use Ansible but all of them have similar structure. If you know how to use one, you will be able to use the rest.

__Orchestration__

Once we've created the templates for all the parts of the system, we need orchestration tools and scripts that talk to the cloud to pull them together into the architecture. Tools:
	
	- CloudFormation (AWS)
	- Ansible (yep it can do this too)
	- Terraform (more to create the VPC, Subnets, etc)

With ansible (powerful tool) you can do both (configuration and orchestration), but we will use ansiblle only for configuration management, and for orchestration we will use terraform.

### Ansible

- Ansible is an open-source software provisioning, configuration management, and application-deployment tool enabling infrastructure as code.

- Ansible is an IT automation engine that automates cloud provisioning, configuration management, application deployment, intra-service orchestration and more.

- It is a __automation__ tool. Configuration management and orchestration tool that helps us autome the task that we usually do manually.

It works only on Mac and Linux.

__Key benefits/Why Ansible__

- __Simple:__ uses YAML (Yet Another Markup Language). So is simple because it uses YAML which is human readble.

- __Agentless:__ That is, when you create a controller, it contains the Ansible controller, but it does not require any configuration tool installation on the nodes it controls. Makes it lightweight on agent nodes, no need to install in agent notes. So for example you can install the ansible tool in the controller node (let's say master node), so then you will not need to do the same in the agent nodes, they don not need to have ansible tool installed.

- __Secure:__ uses SSH to connect to other servers.

- __Efficient SDLC:__ Integrates with other tools, so we can change provider very easily. If you write a script for one platform (aws) then you can easily switch to another (azure). All we need to do is change the cloud provider, that's it. With integration, you can use ansible with docker for example.

- It is a __automation__ tool. Configuration management and orchestration tool that helps us autome the task that we usually do manually.

- You don't need to have previous experience/knowledge of scripting.

![ANSIBLE](./Ansible_scheme.png)

A managed node is any device being managed by the control node. Ansible works by connecting to nodes (clients, servers, or whatever you're configuring) on a network, and then sending a small program called an Ansible module to that node. Ansible executes these modules over SSH and removes them when finished.

We are going to run three virtual machines with vagrant. The configuration file can be found in this repository `vagrantfile`.

Vagrantfile with 3 VMs configuration: the controller, the web and the database servers.

After spinning up the machines `vagrant up`, we can ssh to the Controller running `vagrant ssh controller`.

Let's install Ansible inside the controller:

````bash
sudo apt-get upgrade -y

sudo apt-get update -y

sudo apt-get install software-properties-common -y

sudo apt-add-repository ppa:ansible/ansible -y

sudo apt-get update -y

sudo apt-get install ansible -y

````

After the installation, we can check if ansible was installed correctly running this command `ansible --version`

After than let's ping to the other two machines (web and db). We will do running `ping ip_web` and `ping ip_db`. All the types should be available in the vagrantfile.

We can access from the controller to the web or db machines using ssh: `ssh vagrant@ip_web` or `ssh vagrant@ip_db`. The password is `vagrant`.

Let's install the tree command: `sudo apt-get install tree -y`. The tree is a tiny, cross-platform command-line program used to recursively list or display the content of a directory in a tree-like format.

In the `/etc/ansible/hosts`, we will need to add the connections. Let's open the file `sudo nano /etc/ansible/hosts` and added this lines:

````
[web]
ip_of_web_machine ansible_connection=ssh ansible_ssh_user=vagrant ansible_ssh_pass=vagrant

````

We can check if now ansible has a connection with the server running the `ping` module: `ansible -m ping web`.

If the ping works, we would receive a message back with success status in green letters.
