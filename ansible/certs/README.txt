This folder contains the certificates and keys that are used for the
OpenVPN server and clients.

The keys are encrypted with Ansible Vault.  To use, include the --vault-id
option to run the playbook, as in

  ansible-playbook --vault-id barc@~/private-keys/barc-vault setup-openvpn.yml

... where the vault-id is 'barc@' and the path to the file where the vault
passwords are stored.
