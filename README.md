# Automations With Ansible Using Role

In this example I am going to use Role to automate the installation of Wordpress on two VMs. Using this role we could add more VMs if we want but for this example I am going to use three:

acs: is going to be the responsible of manage all automations and run it to affect as many Vms as we want.

web: this VM is going to host the web server Apache.

db: this is the server that host the MySql instance.

Here is how it looks like:

![This is the architecture of VM we are going to use](https://raw.githubusercontent.com/edgarleonardo/Ansible_Example/master/Network_Architecture.jpg)



## A Bit Explanation about using Role in Ansible

A role is a more simple way of how we can model our datacenter process for automations, no matter it runs on cloud or on premise.

Using roles we are able to organize the tasks using predifined directory name allowing us understanding the blocks of automations easier and the sharing of those process easier as well.

For example, imagin we need to prepare a server to work as builder of our applications:

**Roles**
- **Builder**
- -- name: install jdk
- -- name: install git

The file structure will end up like I show above.

## Which roles we are going to manage?

For this example we are going to create three roles that will run the automations on the servers **web** and **db**, and are going to be installed from **acs** that is the server responsible to make all those installations.

The roles has an specific file structure as follow, so, instead of using tasks, variables and handlers, we're going to use directory:

- **roles**
- -- role
- ------defaults
- - ----files
- - ----handlers
- - ----meta
- ------tasks
- ------templates
- ------vars

Inside folders vars, tasks and handlers we need to add a main.yml if we need to create one of those in our Ansible role running.

In this example there are three roles:

**dbserver**: this install the related package for run mysql instance.
**server-common**: this role install the related packages among the db and web server.
**webserver**: this install all component needed to run Apache.

The file site.yml is meant to has a big picture of all process configure in the role.

In order to run the whole role or an specific one we just do what is next:

This will run the entire role's tasks:

    $ ansible-playbook site.yml

This will run only the db related tasks:

    $ ansible-playbook site.yml -tag "dbserver"

This will run only the web related tasks:

    $ ansible-playbook site.yml -tag "webserver"
