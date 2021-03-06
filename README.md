Docker Zabbix
========================

## Container 

The container provides:

A *Zabbix Servervices* please refer to the [Zabbix documentation](http://www.zabbix.com/) for additional info.

* A *Zabbix Server* at port 10051.
* A *Zabbix Web UI* at port 80 (e.g. `http://$container_ip/zabbix` )
* A *Zabbix Java Gateway* port 10052. 
* A *Zabbix Agent*.
* An SSHD server, user should be `root` and has no password.
* A MySQL supporting *Zabbix*, user is `zabbix` and password is `zabbix`.
* A monit deamon managing the processes. (http://$container_ip:2812, user 'myuser' and password 'mypassword')

## Usage

As an example you can run Zabbix as a service executing the following command.

```
docker run -d -p 10051:10051 -p 10052:10052 -p 80:80 -p 2812:2812 -p 22 berngp/docker-zabbix
```

Such command will expose the *Zabbix Server* through port *10051* and the *Web UI* through port *80* on the host instance. 
Be patient, it takes a minute or two to configure the MySQL instance and start the proper services. You can tail the logs using `docker logs -f $contaienr_id`.

After the container is ready you can get to the *Zabbix Web UI* through `http://$container_ip/zabbix`. User is `admin` and password is `zabbix`.

# Developers

## Setting your Docker environment with the Vagrantfile 

To run the included _Vagrantfile_ you will need [VirtualBox](https://www.virtualbox.org/) and [Vagrant](http://www.vagrantup.com/) installed. Currently I am testing it against _VirtualBox_ 4.3.6 and _Vagrant_ 1.4.1. The _Vagrantfile_ uses a minimal _Ubuntu Precise 64_ box and installs the _VirtualBox Guest Additions_ along with _Docker_ and its dependencies. The first time you execute a `vagrant up` it will go through an installation and build process, after its done you will have to execute a `vagrant reload`. After that you should be able to do a `vagrant ssh` and find that _Docker_ is available using a `which docker` command. 

*Be aware* that the _Vagrantfile_ is depends on the version of _VirtualBox_ and may run into problems if you don't have the latest versions.

Once your _Vagrant_ instance is up you should be able to ssh in (`vagrant ssh`) and have the `docker` command available. By default the _Docker_ is also started as a service/daemon. 

## Building the Docker Zabbix Repository.

Within an environment that is already running _Docker_, such as the _VirtualBox_ instance described above, checkout the *docker-zabbix* code to a known directory. If you are using the _Vagrantfile_ it will be available by default in the `/docker/docker-zabbix` directory. From there you can execute a build and run the container.

e.g. 

```
# CD into the docker container code.
cd /docker/docker-zabbix
# Build the contaienr code.
docker build -t berngp/docker-zabbix .
# Run it!
docker run -i -t -p 10051:10051 -p 10052:10052 -p 80:80 -p2812:2812 berngp/docker-zabbix
```

## Exploring the Docker Zabbix Container

Somtimes you might just want to review how things are deployed iniside the container. You can do that by boostrapping the container and jumping into a _bash shell_. Execute the command bellow to do it.

```
docker run -i -t -p 10051 -p 10052 -p 80 -p 2812 --entrypoint="" berngp/docker-zabbix /bin/bash
```

Note that in the example above we are telling _docker_ to bind to ports 10051 and 80 but we are not giving explicit ports to bind to. You will have to run `docker ps` to figure out the port mappings in relationship with the host.


Happy metrics gathering!
