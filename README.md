# ansible-digitalocean-vpn
Ansible role for creating personal VPN via DigitalOcean.

## Requirements
You should have Ansible installed. See [below for installation details](#installing-ansible).

## VPN server
Creates a DigitalOcean droplet for use as an OpenVPN server.
Also configures your localhost as a VPN client and activates
the connection via NetworkManager.

Usage:
```
./create-vpn
```

If you already use Ansible to manage systems, then you don't
need the `create-vpn` wrapper script, which handles creating
dynamic inventories. Instead, just include the `digitalocean.yml`
playbook, or run it directly:

```
ansible-playbook digitalocean.yml
```

## OpenVPN server
Spins up a DigitalOcean droplet named `vpn` and configures it as an
OpenVPN server. If a droplet named `vpn` already exists, then the script will use that,
and skip creating the droplet.

If you have NetworkManager installed (particularly `nmcli`), then the VPN connection
will be automatically activated. If you don't have NetworkManager, use the
OpenVPN config file at `openvpn_files/client.ovpn` to import into whatever
you use to manage network settings.

After running the playbook, check that you are really using VPN: `curl ipecho.net/plain`

## DigitalOcean API

In order to provision DigitalOcean droplets, you'll need to configure
API access to DigitalOcean. Make sure to use v2 of the API.

Export the following environment variables on the VPN client:

```
DO_API_TOKEN    # v2_api_token_here
DO_SSH_KEY_ID   # integer slug for SSH key saved in DigitalOcean
DO_SSH_KEY_FILE # local path to SSH private key on disk, defaults to ~/.ssh/digital_ocean
```

Once you've exported `DO_API_TOKEN`, you can find the other values via the
`digital_ocean.py` inventory script shipped with Ansible.

## Installing Ansible
If you haven't used Ansible before, you'll need to install it to use this script.
On most platforms, the simplest way is to use pip:

```
pip install --upgrade ansible
```

On Debian/Ubuntu, consider using the PPA, so updates are installed
automatically as part of the system-wide package management:

```
sudo apt-add-repository ppa:ansible/ansible
sudo apt-get update
sudo apt-get install ansible
```

## Contributing

Pull requests are welcome. Thanks to Timur Batyrshin for the original version.

## License and Author

* Authors:: Timur Batyrshin, Conor Schaefer
* License:: Apache 2.0
