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

Export the following environment variables on the VPN client:

```
DO_API_TOKEN    # v2_api_token_here
DO_SSH_KEY_ID   # integer slug for SSH key saved in DigitalOcean
DO_SSH_KEY_FILE # local path to SSH private key on disk, defaults to ~/.ssh/digital_ocean
```

Once you've exported `DO_API_TOKEN`, you can find the other values via the
`digital_ocean.py` inventory script shipped with Ansible.


## Contributing

Pull requests are welcome. Thanks to Timur Batyrshin for the original version.

## License and Author

* Authors:: Timur Batyrshin, Conor Schaefer
* License:: Apache 2.0
