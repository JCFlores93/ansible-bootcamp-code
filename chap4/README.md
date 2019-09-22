ansible-console --help
//commands
ansible all -m ping 
ansible * -m ping 
ansible all -m ping  --limit lb
ansible app1:app2 -m ping
//debemos crear un grupo primero
ansible prod -m ping 
//Todo menos db
ansible 'prod:!db' -m ping
//todo menos lb y db
ansible 'prod:!db:!lb' -m ping
//slicing
ansible 'prod[0:2]' -m ping
ansible '~(app|db).*' -m ping
ansible all -a hostname
// How long the hosts are up?
ansible all -a uptime
//Check memory info on app servers
ansible app -a free
// Installing packages
ansible app -a "yum install -y docker-engine"
// as sudo
ansible app -s -a "yum install -y docker-engine"
ansible app -b -a "yum install -y docker-engine"
//Running commands one machine at a time
ansible all -f 1 -a "free"
//To create a group
ansible app -s -m group -a "name=admin state=present"
// To create a user
ansible app -s -m user -a "name=devops group=admin createhome=yes"
//Copy a file using copy modules
ansible app -m copy -a "src=/vagrant/test.txt dest=/tmp/test.txt"
//Installing  vim Using Modules
ansible lb -m yum -b -a "name=vim state=present"
ansible db -m yum -b -a "name=vim state=present"
ansible prod -m group -b -a "name=admin state=present gid=7045"
ansible 'prod:!db' -m user -b -a "name=abc state=present uid=7045 group=admin"
ansible app -m copy -a "src=test.txt dest=/tmp/test.txt node=0644"
ansible prod -m command -a "free | grep -i swap"
ansible prod -m shell -a "free | grep -i swap"
ansible prod -vvv -m raw -a "free | grep -i swap"
// Create directories on remote nodes
ansible app -m command -a "mkdir /tmp/dir1"
ansible app -m command -a "mkdir /tmp/dir1 create=/tmp/dir1"

ansible-playbook systems.yml --syntax-check
ansible-playbook systems.yml --list-hosts
ansible-playbook systems.yml --list-tasks
ansible-playbook systems.yml --list-tags
ansible-playbook systems.yml --check
ansible-playbook systems.yml
ansible-playbook systems.yml --step

//run a specific task
ansible-playbook systems.yml --start-at-task="create a deploy user"

ansible-playbook systems.yml --limit app

// Roles help to us to make modular and simplify
// tasks -> actions to take, using modules
// defaults -> default properties/vars
// vars -> role specific vars
// files -> static files to be copied to nodes
// templates -> files generated dynamically
// handlers -> actions to take on events
// tests -> ansible tests
// meta -> role meta data
// role : app
//  1   :  1

// init 
//ansible-galaxy init --offline --init-path=roles apache

// check httpd
// ansible app -b -a "which httpd"
// ansible app -b -a "service httpd status"

ansible all -m setup -a "filter=ansible_os_family"
ansible-galaxy install geerlingguy.haproxy
ansible lb -a "service haproxy status"


ansible-galaxy install geerlingguy.mysql
ansible-playbook db.yml
ansible db -b -a "service mysqld status"

ansible db -b -a "/sbin/ifconfig eth0"