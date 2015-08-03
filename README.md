# ansible-digitalocean-vpn
Ansible role for creating personal VPN via DigitalOcean.

## Requirements
You should have the following already installed:

* Ansible
* NetworkManager

## VPN server
Creates a DigitalOcean droplet for use as an OpenVPN server.
Also configures your localhost as a VPN client and activates
the connection via NetworkManager.

Usage:
```bash
./create_vpn.py
```

## OpenVPN server

Spins up a DigitalOcean droplet named `vpn` and configures it as an
OpenVPN server. If a droplet named `vpn` already exists, then the script will use that,
and skip creating the droplet.

After running the playbook, check that you are really using VPN: `curl ipecho.net/plain`

## DigitalOcean API

In order to provision DigitalOcean droplets, you'll need to configure API access to DigitalOcean.

You can generate your client ID and API key at https://cloud.digitalocean.com/api_access
Unfortunately the Ansible digitalocean dynamic inventory module only supports the
deprecated v1 of the DigitalOcean API.

Export the following environment variables on the VPN client:

```
DO_API_KEY      # v1_api_token_here
DO_CLIENT_ID    # v1_api_client_id_here
DO_SSH_KEY_ID   # integer slug for SSH key saved in DigitalOcean
DO_SSH_KEY_FILE # local path to SSH private key on disk
```
```
do_api_key: "{{ansible_env.DO_API_KEY}}"
do_client_id: "{{ansible_env.DO_CLIENT_ID}}"
do_ssh_key_id: "{{ansible_env.DO_SSH_KEY_ID}}"
do_ssh_key_id: "{{ansible_env.DO_SSH_KEY_FILE}}"
```


As for ssh key id, you can only know that by doing manual API request:

```yaml
---
do_client_id: YOUR_CLIENT_ID_HERE
do_api_key: YOUR_API_KEY_HERE
do_ssh_key_id: SSH_KEY_ID
```

```
curl -k 'https://api.digitalocean.com/ssh_keys/?client_id=YOUR_CLIENT_ID&api_key=YOUR_API_KEY'
```
   and check the number in "id" field.

* Make sure you can login to your localhost by SSH as yourself or root

When you have the above in place you run the command:
```
./create_vpn.sh
```
and have the VPN server accessible as `vpn` hostname from your localhost in a several moments.

**Important:** If you use `ansible-playbook` instead and you don't have passwordless sudo for yourself
set up on localhost you must run the command with `-K` key (like `ansible-playbook -K vpn_digital_ocean.yml`)
or ansible will hang infinitely.


## Contributing

Pull requests are welcome

## License and Author

* Author:: Timur Batyrshin
* License:: Apache 2.0
