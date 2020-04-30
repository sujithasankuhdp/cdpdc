# IoT Predictive Maintenance Workshop

## Requirements

Quick guide to install Ansible is [here](https://gist.github.com/fabiog1901/bbc22d91a556e16ebae8b1a90e1881fc).

1. Ensure Ansible runs on Python 3 (not the default on OSX), see section below.

2. Run `ssh-agent`, and `ssh-add` the ssh private key.

    ```bash
    $ ssh-agent
    SSH_AUTH_SOCK=/tmp/ssh-miJvhF3rsjQn/agent.91002; export SSH_AUTH_SOCK;
    SSH_AGENT_PID=91003; export SSH_AGENT_PID;
    echo Agent pid 91003;

    $ ssh-add ~/fusion.pem
    Identity added: /home/fabio/fusion.pem (/home/fabio/fusion.pem)

    $ ssh-add -l
    2048 SHA256:trstgLzdLS/2EE7Hgrerewb8JmBn2herwsVgfrsdxkQ /home/fabio/fusion.pem (RSA)
    ```

3. Create a directory `/opt/parcels-mirrors` and put the Minifi and EFM tarballs in it:

    ```bash
    $ ll /opt/parcels-mirror/
    -rw-r--r--. 1 fabio fabio  59M Oct 11 13:30 efm-1.0.0.1.1.0.0-172-bin.tar.gz
    -rw-r--r--. 1 fabio fabio 138M Oct 11 13:30 minifi-0.6.0.1.1.0.0-172-bin.tar.gz
    ```

4. Export Openstack variables to your environment.

    ```bash
    export OS_REGION_NAME=RegionOne
    export OS_INTERFACE=public
    export OS_AUTH_URL=https://api-fusion.l42.cloudera.com:5000/v3
    export OS_USERNAME=fabio.ghirardello
    export OS_PROJECT_ID=b07765cd6ccf4530a74bcc007d0b55ed
    export OS_USER_DOMAIN_NAME=CLDR
    export OS_PROJECT_NAME=SE-GOES
    export OS_PASSWORD=my-password
    export OS_IDENTITY_API_VERSION=3
    ```

5. In the Ansible configuration file, usually at `/etc/ansible/ansible.cfg`, ensure this is set:

    ```bash
    [default]
    host_key_checking = False
    ```

## How to manually run the playbook

```bash
ansible-galaxy install -r roles/requirements.yml -p roles/

ansible-playbook site.yml \
    -e "public_key=fusion" \
    -e "deployment_id=snconfield" \
    -e "owner=fabio.ghirardello" \
    -e "end_date=03312020" \
    -e "project_name=snconfield" \
    -e "domain=se-goes.fuse.l42.cloudera.com" \
    -e "subnet=SE-GOES_INTERNAL_NET"

    # if you use OSX, you might have to define the correct py3 interpreter location
    #-e "ansible_python_interpreter=/usr/local/bin/python3"
```
