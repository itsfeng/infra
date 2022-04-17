# infra

## Prerequisites
- Debian Install
- expecting user with given name (see private host_vars)
- expecting ssh on default port
- sudo installed
  - passwordless sudo enabled
  - replace sudo statement in /etc/sudoers with:
  - `%sudo ALL=(ALL) NOPASSWD: ALL`
  - add normal user to sudo group:  `usermod -a -G sudo fng`
- volume mounts are not done in here

## Usage
Create a host varialbe file and adjust the variables:
```
cd infra/ansible
mkdir -p host_vars/$YOUR_HOSTNAME
vi host_vars/$YOUR_HOSTNAME/vars.yml
```
or get the non-pub files from secrets store

Create an encrypted `secret.yml` file and adjust the variables:
```
ansible-vault create host_vars/$YOUR_HOSTNAME/secret.yml
ansible-vault edit host_vars/$YOUR_HOSTNAME/secret.yml
```

Add your custom inventory file to `hosts`:
```
cp hosts_example hosts
vi hosts
```

Install the dependencies:
```
ansible-galaxy install -r requirements.yml
```

Finally, run the playbook:
```
ansible-playbook run.yml -l $YOUR_HOSTNAME -K
```
The "-K" parameter is only necessary for the first run, since the playbook configures passwordless sudo for the main login user

For consecutive runs, if you only want to update the Docker containers, you can run the playbook like this:
```
ansible-playbook run.yml --tags="port,containers"
```


