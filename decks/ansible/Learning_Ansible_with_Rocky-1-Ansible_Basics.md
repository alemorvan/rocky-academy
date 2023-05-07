---
marp: true
theme: gaia
_class: lead
paginate: true
markdown.marp.enableHtml: true

backgroundColor: #fff

header: Learning Ansible with Rocky | 1 - Ansible Basics
footer: Rocky Linux Academy - Ansible courses
---
<style>
img[alt~="center"] {
  display: block;
  margin: 0 auto;
}
blockquote {
  background: #ffedcc;
  border-left: 10px solid #d1bf9d;
  margin: 1.5em 10px;
  padding: 0.5em 10px;
}
blockquote:before{
  content: unset;
}
blockquote:after{
  content: unset;
}
header {
    display: grid;
    grid-template-columns: 1fr max-content;
    background-color: #10b981;
    align-content: right;
    color: white;
    font-size: 1em;
    padding: 20px;
}
footer {
    display: grid;
    grid-template-columns: 1fr max-content;
    background-color: #10b981;
    align-content: right;
    color: white;
}

.columns {
  display: grid;
  grid-template-columns: repeat(2, minmax(0, 1fr));
  gap: 1rem;
}
.columns3 {
  display: grid;
  grid-template-columns: repeat(3, minmax(0, 1fr));
  gap: 1rem;
} 

.fa-twitter { color: aqua; }
.fa-mastodon { color: purple; }
.fa-linkedin { color: blue; }
.fa-window-maximize { color: skyblue; }
.fa-circle-exclamation { color: red; }
.fa-trophy { color: #10b981; }
@import 'https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.3.0/css/all.min.css'
table {
  font-size: 10px;
}
</style>

# 1 - Ansible Basics
## Learning Ansible with Rocky

---
<br/>

# <i class="fa-solid fa-trophy"></i> Objectives

In this chapter you will learn how to work with Ansible.

* Implement Ansible;
* Apply configuration changes on a server;
* Create first Ansible playbooks;
---
#

Ansible centralizes and automates administration tasks. It is:

* agentless
* idempotent
* SSH Protocol (Linux), WinRM Protocol (Windows), API calls

---
#

<br/>
<br/>
<br/>

<i class="fa-solid fa-circle-exclamation"></i> The opening of SSH or WinRM flows to all clients from the Ansible server, makes it a **critical element of the architecture** that must be carefully monitored.



---
#

Ansible can manage differents OS, network equipments, and even containers!

* Linux,
* Windows,
* Docker,
* Network (junos, fortios, ...)
* ...

---
### Ansible functionalities

<div class="columns">
<div>

![right:50% w:500](./assets/chapter1/life-cycle.png)

</div>
<div>

Ansible is "push-based" (stateless).

</div>
</div>

---

### A little word about devops

<div class="columns">

<div>
Optimize the work of all the teams:

  * build
  * run
  * change
</div>
<div>

![right:20% w:250](./assets/chapter1/devops.png)
</div>

</div>
implementing continuous integration/deployment

---
#

<br/>
<br/>
<br/>

To offer a graphical interface to your daily use of Ansible, you can install some tools like Ansible Tower (RedHat), which is not free, its opensource counterpart Awx, or other projects like Jenkins and the excellent Rundeck can also be used.

---
### The Ansible vocabulary

The management machine is the server on which Ansible is installed (no software is deployed on the managed servers.).

<div class="columns">
<div>

* task
* module
* inventory
* playbook
</div>
<div>

* role
* fact
* handler
* collection

</div>
</div>

<!-- 
The management machine: the machine on which Ansible is installed. Since Ansible is agentless, no software is deployed on the managed servers.
The inventory: a file containing information about the managed servers.
The tasks: a task is a block defining a procedure to be executed (e.g., create a user or a group, install a software package, etc.).
A module: a module abstracts a task. There are many modules provided by Ansible.
The playbooks: a simple file in yaml format defining the target servers and the tasks to be performed.
A role: a role allows you to organize the playbooks and all the other necessary files (templates, scripts, etc.) to facilitate the sharing and reuse of code.
A collection: a collection includes a logical set of playbooks, roles, modules, and plugins.
The facts: these are global variables containing information about the system (machine name, system version, network interface and configuration, etc.).
The handlers: these are used to cause a service to be stopped or restarted in the event of a change.
-->


---
<br/>
<br/>
<br/>


# Questions ?

---
<br/>
<br/>
<br/>

# Installation on the management server

---
#
Ansible is available in the repositories of the main distributions.

On Rocky 9 / RHEL 9 (EPEL repo) :

```bash
sudo dnf install epel-release
sudo dnf makecache
sudo dnf install ansible 
```

On Debian (Launchpad repo) :

```bash
sudo apt-get install ansible
```

---
# Configuration files

The server configuration is located under `/etc/ansible`

The main configuration file (commands, modules, plugins, and ssh configuration):

```bash
/etc/ansible/ansible.cfg
```

The inventory file: (declaration of clients and groups):

```bash
/etc/ansible/hosts
```

---
# The main configuration file

`ansible-config` command generate a new configuration file:

```bash
usage: ansible-config [-h] [--version] [-v] {list,dump,view,init} ...

View ansible configuration.

positional arguments:
  {list,dump,view,init}
    list                Print all config options
    dump                Dump configuration
    view                View configuration file
    init                Create initial configuration
```

---
# The main configuration file

Example:

```bash
ansible-config init --disabled > /etc/ansible/ansible.cfg
```

---
# The inventory file

The inventory file can be written in different formats (including yaml):

Creation of a `Rocky9` group containing 2 targets:

```bash
[Rocky9]
172.16.1.210
172.16.1.211
```

---

# The inventory file

Creating a group of groups:

```bash
[rocky:children]
rocky8
rocky9
```


---
<br/>
<br/>
<br/>


# Questions ?

---
# ansible command line usage
<style scoped>
table {
  font-size: 0.5em;
}
</style>

The ansible command launches a task on one or more target hosts.

```bash
ansible <host-pattern> [-m module_name] [-a args] [options]
```

| Option | Information |
|--------|-------------|
| -m module	             | Runs the module called|
| -a 'arguments'         | The arguments to pass to the module. |
| -b -K	                 | Requests a password and runs the command with higher privileges. |
| --user=username	       | Uses this user to connect to the target host instead of the current user. | 
| --become-user=username | Executes the operation as this user (default: root). | 
| -C	                   | Simulation. Does not make any changes to the target but tests it to see what should be changed.| 

---
#

<i class="fa-solid fa-circle-exclamation"></i> Since we have not yet configured authentication on our 2 test servers, not all the following examples will work. They are given as examples to facilitate understanding, and will be fully functional later in this chapter.

---
#

List the hosts belonging to the rocky8 group:

```bash
ansible rocky9 --list-hosts
```

---
#

Ping a host group with the ping module:

```bash
ansible rocky9 -m ping
```

---
#

Display facts from a host group with the setup module:

```bash
ansible rocky9 -m setup
```

---
#

Run a command on a host group by invoking the command module with arguments:

```bash
ansible rocky9 -m command -a 'uptime'
```

---
#

Run a command with administrator privileges:

```bash
ansible rocky9 --become -m command -a 'reboot'
```

---
#

Run a command using a custom inventory file:

```bash
ansible rocky9 -i ./local-inventory -m command -a 'date'
```

> As in this example, it is sometimes simpler to separate the declaration of managed devices into several files (by cloud project for example) and provide Ansible with the path to these files, rather than to maintain a long inventory file.

---
# Preparing the client

On both management machine and clients, we will create an ansible user dedicated to the operations performed by Ansible. This user will have to use sudo rights, so it will have to be added to the wheel group.

---
# Preparing the client

This user will be used:

* On the administration station side: to run ansible commands and ssh to managed clients.
* On the managed stations (here the server that serves as your administration station also serves as a client, so it is managed by itself) to execute the commands launched from the administration station: it must therefore have sudo rights.

---
# Preparing the client

On both machines, create an ansible user, dedicated to ansible:

```bash
$ sudo useradd ansible
$ sudo usermod -aG wheel ansible
```

Set a password for this user:

```bash
$ sudo passwd ansible
```

---
# Preparing the client

Modify the `sudoers` config to allow members of the `wheel` group to sudo without password:

```bash
$ sudo visudo
```

---
# Preparing the client

Our goal here is to comment out the default, and uncomment the `NOPASSWD` option so that these lines look like this when we are done:

```bash
## Allows people in group wheel to run all commands
# %wheel  ALL=(ALL)       ALL

## Same thing without a password
%wheel        ALL=(ALL)       NOPASSWD: ALL
```

---
# Test with the ping module

By default, password login is not allowed by Ansible.

Uncomment the following line from the `[defaults]` section in the `/etc/ansible/ansible.cfg` configuration file and set it to `True`:

```bash
ask_pass      = True
```

---
# Test with the ping module

When using management from this point on, start working with this new user:

```bash
$ sudo su - ansible
```

---
# Test with the ping module

Run a ping on each server of the rocky8 group:

```bash
SHELL > ansible rocky9 -m ping
SSH password:
172.16.1.10 | SUCCESS => {
    "changed": false,
    "ping": "pong"
}
172.16.1.11 | SUCCESS => {
    "changed": false,
    "ping": "pong"
}
```

---
#

You are asked for the ansible password of the remote servers, which is a security problem...

You can now test the commands that didn't work previously in this chapter.

---
# Key authentication

Password authentication will be replaced by a much more secure private/public key authentication.

---
# Creating an SSH key

The dual-key will be generated with the command ssh-keygen on the **management station by the ansible user**:

```bash
SHELL > ssh-keygen
Generating public/private rsa key pair.
Enter file in which to save the key (/home/ansible/.ssh/id_rsa):
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in /home/ansible/.ssh/id_rsa.
Your public key has been saved in /home/ansible/.ssh/id_rsa.pub.
The key fingerprint is:
SHA256:Oa1d2hYzzdO0e/K10XPad25TA1nrSVRPIuS4fnmKr9g ansible@localhost.localdomain
The key's randomart image is:
+---[RSA 3072]----+
|           .o . +|
|           o . =.|
|          . . + +|
|         o . = =.|
|        S o = B.o|
|         = + = =+|
|        . + = o+B|
|         o + o *@|
|        . Eoo .+B|
+----[SHA256]-----+
```

---
# Creating an SSH key

The public key can be copied to the servers:

```bash
# ssh-copy-id ansible@172.16.1.10
# ssh-copy-id ansible@172.16.1.11
```

Re-comment the following line from the `[defaults]` section in the `/etc/ansible/ansible.cfg` configuration file to prevent password authentication

```bash
# ask_pass      = True
```
---

# Private key authentication test

For the next test, the shell module, allowing remote command execution, is used:

```bash
# ansible rocky8 -m shell -a "uptime"
172.16.1.10 | SUCCESS | rc=0 >>
 12:36:18 up 57 min,  1 user,  load average: 0.00, 0.00, 0.00

172.16.1.11 | SUCCESS | rc=0 >>
 12:37:07 up 57 min,  1 user,  load average: 0.00, 0.00, 0.00
```
No password is required, private/public key authentication works!

---
# 

<i class="fa-solid fa-circle-exclamation"></i> In production environment, you should now remove the `ansible` passwords previously set to enforce your security (as now an authentication password is not necessary).

---
# The modules

The list of modules classified by category can be found [here](https://docs.ansible.com/ansible/latest/collections/index_module.html). 

Ansible offers more than 750!

The modules are now grouped into module collections, a list of which can be found [here](https://docs.ansible.com/ansible/latest/collections/index.html).

> Collections are a distribution format for Ansible content that can include playbooks, roles, modules, and plugins.





