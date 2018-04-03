# TrueOS Poudriere setup 

Experimental Ansible playbook for deploying basic poudriere port building machine. 

 

You may need it if you want:
 
 - to build packages for TrueOS in the same env as official
 - to make patches for existing software 
 - to port something new


### Usage
 
#### Requirements 
- FreeBSD/TrueOS box accessible via ssh. (VM, remote sever or even localhost) 
- Root password  for that host (for jails management)
- Ansible
 
 install it through pip/pyenv/pipenv or:
  
    sudo pkg install ansible
    

#### Installation
localhost: `sudo ansible-playbook  chimaerlet.yml --connection=local` 

other hosts: set your host variables in `.ansible/hosts`  then run:

    `ansible-playbook chimaerlet.yml --extra-vars "phosts=trueos.example.com"`  

(replace `trueos.example.com` with your box name)

#### Running 

Login to your box and run:
- to test: `sudo poudriere bulk  -j trueos -p trueos-ports games/oldrunner` 
- to build all packages: `sudo poudriere bulk  -j trueos -p trueos-ports -a`


#### Customization
some settings can be overriden like this:
    
    `ansible-playbook chimaerlet.yml --extra-vars "phosts=localhost"`

other default values:

    `url_conf: "{{ url_conf | default('https://raw.githubusercontent.com/trueos/trueos-core/master/build-files/conf/desktop/port-make.conf') }}"`

    `url_dist: "{{ url_dist | default('http://download.trueos.org/unstable/amd64/dist/') }}"` 
    
#### Todo:

This is part of a larger [task](https://github.com/trueos/trueos-core/issues/1537)
Made using instructions by @kmoore134      