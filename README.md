# TrueOS Poudriere setup 

Experimental Ansible playbook for deploying basic poudriere port building machine


### Usage
 
#### Requirements 
- FreeBSD/TrueOS box accessible via ssh. (VM, remote sever or even localhost) 
- Root password  for that host (for jails management)
- Ansible
 
 install it through pip/pyenv/pipenv or:
  
    sudo pkg install ansible
    

#### Installation
Set your host variables in `.ansible/hosts`  then run:

`ansible-playbook chimaerlet.yml --extra-vars "phosts=localhost"`  (replace `localhost` with your box name)

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