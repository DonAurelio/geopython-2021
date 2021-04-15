# Introduction to the Open Data Cube: GeoPython 2021

Welcome to this repo, here you will find all the material related to the talk.

1. Presetation
2. References
3. Deploy your own Data Cube

## Presenation

Feel free to take at look at the `Presentation.pdf` file.

## References

We cover from 1 to 5 during the talk. Reference 6 have more introductory notebooks from the DEA datacube. Finally, there are amazing Geoscience-BigData related projects that may be interesting to explore, references 7 and 8 cover one of these projects. 

1. [What is the Open Data Cube?](https://medium.com/opendatacube/what-is-open-data-cube-805af60820d7) 
2. [Imaging the Past](https://landsat.gsfc.nasa.gov/article/imaging-past)
3. [Open Data Cube Web Page](https://www.opendatacube.org/)
4. [Open Data Cube Manual](https://datacube-core.readthedocs.io/en/latest/)
5. [Cube in a Box](https://www.opendatacube.org/ciab)
6. [Digital Earth Australia notebooks and tools repository](https://github.com/GeoscienceAustralia/dea-notebooks)
7. [Pangeo: A community platform for Big Data geoscience](https://pangeo.io/)
8. [Pangeo History](https://medium.com/pangeo/pangeo-2-0-2bedf099582d)

## Deploy your own Data Cube

Here I will show you how you can deploy your own instance of the open datacube using the configuration files I have prepared for you. So let's do it, I promise it won't be painful.

This configuration file were made to fulfil the purpose of this talk, then if you are considering to continue the exploration of the open data cube use the [Cube in a Box](https://www.opendatacube.org/ciab) alternative.

### Requirements

1. Computer with at least 8 GB of RAM memory.
2. Windows or Linux OS
 
### Preliminary configuration

1. Download and install [Virtualbox 6.1.18](https://download.virtualbox.org/virtualbox/6.1.18/VirtualBox-6.1.18-142142-Win.exe)
2. Download and install [Vagrant 2.2.15](https://releases.hashicorp.com/vagrant/2.2.15/vagrant_2.2.15_x86_64.msi)
3. Reboot your computer

### Deployment of a Virtual Machine with Vagrant

Vangrant is a tool that automates the deployment of virtual machines. All the details and configuration of the virtual machine is already set for me in the `Vagrantfile`, you can take a look on this. On this file I configured an ubuntu based virtual machine, this machine will use two host ports the `8080` for Jupyter Notebooks and `8081` for the open data cube explorer. Make sure this ports are not used by other application on your computer.

We use a virtual machine in order to easy the deployment of containers and avoid other tecnical details that can take place during the deployment, since this virtual machine is already prepared to do that. However, if you are familiar with containers you can follow up to the 4 step and go to the next section to deploy the open data cube containers directly on your system. 

1. Download [geopython-2021-main.zip](https://github.com/DonAurelio/geopython-2021/archive/refs/heads/main.zip) repository.
2. Extract the ZIP file. 
3. Get into the `geopython-2021-main` folder, make sure you can see the `Vagrantfile` on this folder.
4. Being located in the directory `geopython-2021-main`, open a PowerShell console (in Windows) or the Terminal (in Linux).
5. Use the following command in order to start a Virtual Machine (VM) automatically

```bash
vagrant up --provision
```

The `vagrant up --provision` command will perform the following steps: (1) look for the `Vagrantfile` that is located in your current directory, (2) it will read the specifications set on this file in order configure the Virtual Machine for you, (3) Download the ubuntu VM image from the Vagrant Cloud, and (4) run the ubuntu VM on your computer. The `--provision` flag will will tell vagrant to install `docker` and `docker-compose` as is specified in the `Vagrantfile`. Docker and docker-compose are the tools requiered to deploy the open data cube containers.  

**NOTE:** The `--provision` flag is used only the first time you deploy the virtual machine.

#### Vagrant Usefull Commands

* *stop* the virtual machine use `vagrant halt`
* *start* the virtual machine use `vagrant up`
* *enter* the virtual machine console use `vagrant ssh`
* *exit* the virtual machine `exit`
* *destroy* the virtual machine use `vagrant destroy`, if you do this, the next time you need to use the virtual machine, use the `vagrant up --provision` command.

### Deployment of Open Data Cube Containers

Assuming you did step 5 of the previous section, use the `vagrant ssh` to get into the virtual machine you created in the previuos section. Then, use the following commands to deploy the open data cube containers.

Get into the `/vagrant` directory

```bash 
vagrant@vagrant:~$ cd /vagrant
```

Check if the `docker-compose.yml` is present in the current directory.

```bash
vagrant@vagrant:/vagrant$ ls -l

total 1780
-rwxrwxrwx 1 vagrant vagrant    1268 Apr 14 23:59 docker-compose.yml
...
```

Start the postgis container

```bash
vagrant@vagrant:~$ sudo docker-compose up -d postgis
```
Start the jupyter container

```bash
vagrant@vagrant:~$ sudo docker-compose up -d jupyter
```

And start the explorer container

```bash
vagrant@vagrant:~$ sudo docker-compose up -d explorer
```

Check that all containers are working properly

```bash
vagrant@vagrant:~$ sudo docker-compose ps
```

Open the browser at [http://localhost:8080](http://localhost:8080) to display the JupyterLab interface and [http://localhost:8081](http://localhost:8081) for the open data cube explorer. If you got "Internal Server Error" in the explorer, don't worry, this is because we don't have indexed images in the data cube database, we will solve this later. 

Follow the instructions given the in the notebooks on this order and enjoy your journey !!

1. Sentinel 2 - Product definition.ipynb
2. Sentinel 2 - Image Indexing.ipynb
3. Sentinel 2 - Normalized Vegetation Index (NDVI).ipynb