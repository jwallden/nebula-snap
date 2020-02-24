# Nebula Snap package

This is an atempt at creating a snap package for the Nebula overlay networking tool.

Current state:

* Nebula binary is running in strict confinement. For this to work you will have to provide:
  * 'config.yaml in /var/snap/nebula'
  * 'ca.crt in /var/snap/nebula/certs'
  * 'nebula-node.crt and nebula-node.key in /var/snap/nebula/certs'
* CA and certificat signing is currently not working. If we continue the current route with certs and config in $SNAP_COMMON a patch for nebula-cert takes care of making -out-crt and -out-key paths to directories will have to be merged. Another solution using the system-files plug could be viable. Will investigate that.
