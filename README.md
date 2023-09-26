# Launch Codes for PerfSonar Setup

### Barebones Server Setup w/ Foreman 
`apt-get -y install ca-certificates`<br>
`cd /tmp && wget https://apt.puppet.com/puppet7-release-focal.deb`<br>
`apt-get install /tmp/puppet7-release-focal.deb`<br>
`wget https://deb.theforeman.org/foreman.asc -O /etc/apt/trusted.gpg.d/foreman.asc`<br>
`echo "deb http://deb.theforeman.org/ focal 3.4" | tee /etc/apt/sources.list.d/foreman.list`<br>
`echo "deb http://deb.theforeman.org/ plugins 3.4" | tee -a /etc/apt/sources.list.d/foreman.list`<br>
`apt-get update && apt-get -y install foreman-installer`<br>
`foreman-installer`<br>

### Barebones Server Setup w/ Cobblerd
`apt-get install cobbler cobbler-web`<br>
**Specific Cobblerd Settings Edits need to be made based on desired system setup - barebones ones are available on their docs page**<br>
`cobbler check`<br>
`cobbler sync`<br>
`mount -o loop ubuntu-server-i386.iso /mnt` <br>
`cobbler import --name=ubuntu-server --path=/mnt --breed=ubuntu` <br>

### Barebones Server Setup w/ Ansible
On Control Node:<br>
`apt-add-repository ppa:ansible/ansible`<br>
`apt update`<br>
`apt install ansible`<br>
`nano /etc/ansible/hosts` (Add Hosts under [servers] i.e. server1 ansible_host=203.0.113.111 and ansible_python_interpreter=/usr/bin/python3 under [all:vars])<br>
`ansible-playbook server-setup.yml -l server1 -u root -k`<br>
On Host Nodes:<br>
`ansible-playbook server-setup.yml -l server1 -u {{ username established in yml file }}`<br>

### PerfSonar TP On Docker Setup on Host Nodes
On Control Node:<br>
`ansible-playbook docker-setup.yml -l server1 -u {{ username established in server-setup yml file }}`<br>

### Esmond Archiver
Needed for Maddash? | Should be handled below by maddash agent

### Host Maddash on Control Node
**Edit ~/unorganized-ansibleps-utils/psconfig-mesh.json per user-required specs**<br>
`cd /etc/apt/sources.list.d/`<br>
`wget http://downloads.perfsonar.net/debian/perfsonar-release.list`<br>
`wget -qO - http://downloads.perfsonar.net/debian/perfsonar-official.gpg.key | apt-key add -`<br>
`apt-get update`<br>
`apt-get install maddash`<br>
`apt-get install perfsonar-psconfig-maddash`<br>
`systemctl start psconfig-maddash-agent`<br>
`psconfig remote add ~/unorganized-ansibleps-utils/psconfig-mesh.json`<br>
`systemctl restart psconfig-maddash-agent`<br>
