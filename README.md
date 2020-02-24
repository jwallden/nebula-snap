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
 If we continue the current route with certs and config in $SNAP_COMMON a patch for nebula-cert takes care of making -out-crt and -out-key paths to directories will have to be merged. Another solution using the system-files plug could be viable. Will investigate that.
