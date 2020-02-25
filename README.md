# Nebula Snap package

This is an atempt at creating a snap package for the Nebula overlay networking tool.

Current state:

* Nebula binary is running in strict confinement. For this to work you will have to provide:
  * `config.yaml in /etc/nebula/config`
  * `ca.crt in /etc/nebula/certs`
  * `nebula-node.crt and nebula-node.key in /etc/nebula/certs`
  * This also means creating the above directories (Hopefully this can be done via an install hook in the future)
* CA creation and certificate signing is working. However, the name of the produced certs are hardcoded to:
  * `ca.crt`
  * `ca.key`
  * `nebula-node.crt`
  * `nebula-node.key`
* Since created certs are placed in /etc/nebula the cert-functionality needs sudo permissions. Not optimal perhaps, but necessary.
 
 If we continue the current route with certs and config hardcoded to /etc/nebula/[config, certs] a patch for nebula-cert that takes care of making -out-crt and -out-key paths to directories will have to be merged. This way we can set our own names on the created certs making it useful if you e g want to create a bunch of certs on one machine and then distribute them to the other hosts in the network later.

## Usage

### Starting Nebula
After placing a config.yaml in /etc/nebula/config you can either start Nebula manually or use the provided daemon

See [here](https://arstechnica.com/gadgets/2019/12/how-to-set-up-your-own-nebula-mesh-vpn-step-by-step/) for instructions on the config file. Also, the [Nebula github page](https://github.com/slackhq/nebula) is a good resource. An example config.yaml can be found there.

#### Start manually:
`sudo nebula`

You can NOT provide a location for the config.yaml file. It is hardcoded to `/etc/nebula/config`

#### Start the daemon:
`sudo snap start nebula.daemon`

:warning: There seems to be an issue with the daemon after a reboot. The daemon is supposed to be started automatically on boot and it gets started. However, Nebula does not get a connection to the lighthouse. A **manual restart of the daemon** fixes this: `sudo snap restart nebula.daemon`

To check if the daemon started as expected:
`sudo snap logs nebula.daemon`

or using systemd:s logging facilities:
`sudo journalctl -r -u snap.nebula.daemon.service`

### Certificate creation

#### Generate a Certificat Authority:

`sudo nebula.cert-ca -name <ORGANIZATION_NAME>`

This will generate `ca.crt` and `ca.key`
Again, paths are hardcoded to `/etc/nebula/certs` so NOT possible to change this at the moment.

#### Generate node certificates and sign them with the above created CA key:

`sudo nebula.cert-sign -name <CLIENT_NAME> -ip <CLIENT_IP_ADDRESS>`

This will generate `nebula-node.crt` and `nebula-node.key` placed in `/etc/nebula/certs`
