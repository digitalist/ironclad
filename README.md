#TrueOS Poudriere setup 

Experimental Ansible playbook for deploying basic poudriere port building machine

By default it installs to VM host `ironclad` with root password `ironclad` (see `.ansible/hosts` inventory file)


some settings can be overriden like this:
    
    `ansible-playbook chimaerlet.yml --extra-vars "phosts=localhost"`


`url_conf: "{{ url_conf | default('https://raw.githubusercontent.com/trueos/trueos-core/master/build-files/conf/desktop/port-make.conf') }}"`

`url_dist: "{{ url_dist | default('http://download.trueos.org/unstable/amd64/dist/') }}"`
