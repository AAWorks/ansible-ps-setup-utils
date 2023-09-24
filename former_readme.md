# unorganized-ansibleps-utils
Unorganized storage for reusable ps scripts

## Server Setup yml
Prereq:
- Ansible-control must have ssh access to server root - can be done upon server creation (w/ digitalocean)

Functionality:
Install aptitude, which is preferred by Ansible as an alternative to the apt package manager.
Create a new sudo user and set up passwordless sudo.
Copy a local SSH public key and include it in the authorized_keys file for the new administrative user on the remote host.
Disable password-based authentication for the root user.
Install system packages.
Configure the UFW firewall to only allow SSH connections and deny any other requests.

## Docker Setup yml
sets up docker container (ps testpoint) on node preconfiged with (server setup)

### Resources
Ansible with docker:
https://www.digitalocean.com/community/tutorials/how-to-use-ansible-to-install-and-set-up-docker-on-ubuntu-20-04
https://www.youtube.com/watch?v=GfJl0ZRAVDY

Ansible with PS:
https://www.youtube.com/watch?v=9OyJsQB59Yg -much unexplained (good starting ground)
https://github.com/perfsonar
https://www.google.com/url?sa=t&rct=j&q=&esrc=s&source=web&cd=&cad=rja&uact=8&ved=2ahUKEwjit4zYtYj_AhVOkYkEHezRB2MQFnoECBoQAQ&url=https%3A%2F%2Fdocs.perfsonar.net%2F3.5rc2%2Fconfig_mesh.html&usg=AOvVaw1rwAiuBhwTH09c_qh6rvpj
https://www.digitalocean.com/community/tutorials/how-to-use-ansible-to-automate-initial-server-setup-on-ubuntu-20-04#prerequisites - somewhat outdated/doesn't mesh well w ps configs

Apache through Docker:
https://www.digitalocean.com/community/tutorials/apache-web-server-dockerfile

Maddash gui:
https://docs.perfsonar.net/maddash_config_webui.html
