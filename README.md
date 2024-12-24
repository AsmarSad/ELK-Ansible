# ELK-Ansible
This Ansible playbook automates the process of creating a DigitalOcean droplet, configuring the server, and installing the ELK stack (Elasticsearch, Logstash, Kibana). 
## Project Structure
```
.
├── ansible.cfg
├── main.yml
├── README.md
├── roles
│   ├── configure_elk
│   │   ├── defaults
│   │   │   └── main.yml
│   │   ├── handlers
│   │   │   └── main.yml
│   │   └── tasks
│   │       └── main.yml
│   └── provision_droplet
│       ├── defaults
│       │   └── main.yml
│       └── tasks
│           └── main.yml
└── secrets.yml

```
## secrets.yml
Sensitive data such as `do_token` and `ssh_pub_key_path` are securely stored using Ansible Vault.

```
ansible-vault create secrets.yml
```

Inside secrets.yml:

```
do_token: "<digital_ocean_token>"
ssh_pub_key_path: "/path/to/key/file"
```

## First Task: Provisioning Droplet

✅ It will upload the SSH key to DigitalOcean.

✅ It will create a droplet in the nyc3 region with the specified size and image.

✅ It waits for the droplet to become reachable and adds it to the Ansible inventory dynamically.

This part of the playbook is executed on the localhost.

## Second Task: Configure and Install ELK
The second play (`configure_elk`) runs on the newly created droplet (`elk_droplet`) and installs and configures the ELK stack.


✅ Installing necessary dependencies (Java, Elasticsearch, Logstash, Kibana).

✅ Adding the Elastic GPG key and repository.

✅ Installing and starting  Elasticsearch, Logstash, and Kibana.

✅ Configuring Kibana to listen on all interfaces.

This play is executed on the droplet that was dynamically added to the inventory in the first play.

## Running Playbook
```
ansible-playbook --ask-vault-pass main.yml
```

## Result
ELK runs on port 5601
