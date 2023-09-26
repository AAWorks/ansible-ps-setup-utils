# Launch Codes for PerfSonar Setup

### Barebones Server Setup w/ Ansible
On Control Node:<br>
1. `./control-setup.sh`<br>
2. `ssh-keygen`<br>
3. `cat /.ssh/id_rsa.pub` (copy resulting key)<br>
4. `nano /etc/ansible/hosts` (Add Hosts under [servers] i.e. server1 ansible_host=203.0.113.111 and ansible_python_interpreter=/usr/bin/python3 under [all:vars])<br>
On Host Nodes:<br>
5. `./setup.sh`<br> (Add control node key to authed keys by adjusting setup.sh with key copied from step 3)<br>
On Control Node:<br>
6. `ansible-playbook server-setup.yml -l [name of specific host (see step 4) or all] -u root -k`<br>

### Further Server Setup w/ Cobblerd (Allows for easier iptable modification which doesn't seem necessary with docker)
`apt-get install cobbler cobbler-web`<br>
**Specific Cobblerd Settings Edits need to be made based on desired system setup - barebones ones are available on their docs page**<br>
`cobbler check`<br>
`cobbler sync`<br>
`mount -o loop ubuntu-server-i386.iso /mnt` <br>
`cobbler import --name=ubuntu-server --path=/mnt --breed=ubuntu` <br>

### Barebones Server Setup w/ Foreman (Untested)
`apt-get -y install ca-certificates`<br>
`cd /tmp && wget https://apt.puppet.com/puppet7-release-focal.deb`<br>
`apt-get install /tmp/puppet7-release-focal.deb`<br>
`wget https://deb.theforeman.org/foreman.asc -O /etc/apt/trusted.gpg.d/foreman.asc`<br>
`echo "deb http://deb.theforeman.org/ focal 3.4" | tee /etc/apt/sources.list.d/foreman.list`<br>
`echo "deb http://deb.theforeman.org/ plugins 3.4" | tee -a /etc/apt/sources.list.d/foreman.list`<br>
`apt-get update && apt-get -y install foreman-installer`<br>
`foreman-installer`<br>

### PerfSonar TestPoint (Toolkit auto-configs iptables but not needed for docker seemingly) On Docker Setup on Host Nodes
On Control Node:<br>
`ansible-playbook docker-setup.yml -l [name of specific host (see step 4) or all] -u root -k`<br>

### Esmond (Legacy) Archiver (PerfSONAR 5.0 no longer suppots the esmond archiver and now runs an OpenSearch based archiver)
Needed for MaDDash<br>

### OpenSearch Archiver
Run local archiving via a perfSONAR toolkit node or this command:<br>
`/usr/lib/perfsonar/archive/perfsonar-scripts/psconfig_archive.sh`<br>
Run archiver on a dedicated host as below:<br>
On Control Node:<br>
`ansible-playbook docker-setup-archiver.yml -l [name of specific host dedicated for archiver] -u root -k`<br>

### Host Maddash on Control Node
**Edit ~/unorganized-ansibleps-utils/psconfig-mesh.json per user-required specs**<br>
Run (1-5) as root:
1. `cd /etc/apt/sources.list.d/`<br>
2. `wget http://downloads.perfsonar.net/debian/perfsonar-release.list`<br>
3. `wget -qO - http://downloads.perfsonar.net/debian/perfsonar-official.gpg.key | apt-key add -`<br>
4. `apt-get update`<br>
5. `apt-get install maddash`<br>
6. `apt-get install perfsonar-psconfig-maddash`<br>
7. `systemctl start psconfig-maddash-agent`<br>
8. `psconfig remote add ~/unorganized-ansibleps-utils/psconfig-basic-network.json`<br>
9. `systemctl restart psconfig-maddash-agent`<br>
**Worked with a modified version of psconfig-basic-network.json focusing on the throughput tests, may need to configure MaDDash grid based on tests scheduled**
