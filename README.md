#Overview
This project is a Vagrant box that is provisioned run various services needed at both build-time
and run-time of a software project.

#Prerequisites

* [Vagrant](https://www.vagrantup.com/) installed and working
* [VirtualBox](https://www.virtualbox.org/) installed and working

#Building
All the components of the environment live in repositories on the internet so there is nothing to build.

#Installation
Type `vagrant up` and go get a cup of coffee.  The construction time of the box greatly depends on your internet speeds.

# Choice Of Disributions
We now support multiple Linux distributions.  If you run `vagrant status` you should see something like this:

```
vagrant status
Current machine states:

xenial-infrastructure             not created (virtualbox)
maipo-infrastructure              not created (virtualbox)
```

By default, `xenial-infrastructure` is the default but you can also run `vagrant up maipo-infrastructure` to run
another distribution.  You can run concurrent instances if you have the hardware and the need.

## Upgrading
Sometimes the Vagrant file changes which can cause some subtle issues, such as creating an orphaned virutal machine.
The safest upgrade procedure is the following:

1. `vagrant destroy` to remove the existing box
1. `git pull` to download the new files
1. **`vagrant box outdated`** to see if newer version of the box is available
1. `vagrant box update --box <boxname>` to pull down the current version of the box
1. `vagrant up` to build the new box

#Tips and Tricks

##RAM and CPU Settings
If you examine the `vagrantfile` file, you will see that the virtual machine is configured to use 2GB of RAM and
1 CPU.  Feel free to change these values to match your computer's hardware.

##Low Disk Space
If an environment is used long enough, it is likely to run out of disk space.  The two main culprits are kernal updates
filling up the `/boot` partition and Docker images filling up the `/var/lib/docker` partition.  You have at least 3 options:

* throw away the environment and start fresh
* clean up the old kernels via `sudo apt autoremove`
* clean up Docker containers via `docker rm --volumes --force $(docker ps --all --quiet)`
* clean up Docker images, after cleaning up the containers, via `docker rmi --force $(docker images --quiet)`

##Verifying The Setup
Log into the system with a username of `vagrant` and password of `vagrant`.

##Installed Infrastructure
* [Docker Engine](https://docs.docker.com/)
* [Docker Compose](https://docs.docker.com/compose/)
* [Docker Machine](https://docs.docker.com/machine/)
* [Ansible](http://www.ansible.com/)
* [Artifactory](https://www.jfrog.com/artifactory/)
* [Elastic Stack](https://www.elastic.co/v5)

##Sub-Projects
The provisioning of the environment is done by several upstream projects.  You might be interested in examining
exactly what they install and get a full inventory of the sofware and conveniences.

* [packer-server-xenial](https://github.com/kurron/packer-server-xenial) - `master` branch
* [packer-server-xenial](https://github.com/kurron/packer-server-xenial) - `infrastructure` branch
* [packer-server-maipo](https://github.com/kurron/packer-server-maipo) - `master` branch
* [packer-server-maipo](https://github.com/kurron/packer-server-maipo) - `infrastructure` branch

#Troubleshooting

## Partial Failure
Sometimes networks fail or mirror sites go down. If you experience a failure, you can attempt to resume the construction
by issuing `vagrant provision` at the command line.  Vagrant will attempt to start over, but will skip any provisions that
have already taken place.

#License and Credits
This project is licensed under the [Apache License Version 2.0, January 2004](http://www.apache.org/licenses/).
