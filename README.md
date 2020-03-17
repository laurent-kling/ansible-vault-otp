vault-otp (vault-ssh-helper) ?
=========

It is a role to configure One-Time SSH Password clients with Hashicorp Vault using vault-ssh-helper.

After applying this role:

* Only login with OTP password work !
* User(s) and root with passwords are invalid !
* SSH logging work as usual.
* *PAM and SSHD configurations are modified*.

This role check:

* Default variables present.
* Hashicorp Vault server active and unseal.
* Coherence between Hashicorp Vault server protocol used and certificate file definition.
* Self-check with the server on the client with vault-ssh-helper binary.

Requirements
------------

* Ubuntu 18.04
* CentOS 7.3

Role Variables
--------------

By default, these 3 variables need to be defined:

1. **vault_addr**: [Required] "http://vault:8200" or "https://vault:8200"

   Use of *https* Vault server need to define the name of the *vault_ca_cert_file*

2. **vault_ca_cert_file**: [Required] "-dev" or "vault.crt"

   - *http* : The default value of "-dev" is only for unsecured Vault servers
   - *https*: The File Name of the PEM-encoded CA certificate file used to verify the Vault server's TLS.

3. **ssh_mount_point**: [Required] "ssh"

   Mount point of SSH backend in the Vault server for SSH OTP.

The additional variables are

* tls_skip_verify: true

  Skip the verification of the certificate, needed if the certificate is self-signed.

* allowed_roles: "*"

  All roles inside the mount point are allowed to login.

* allowed_cidr_list: "0.0.0.0/0"

  By default allowed all IP to connect with OTP to the client.

  To be restricted to a [specific subrange](https://www.ipaddressguide.com/cidr) using [CIDR block](https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing#CIDR_notation) notation.

Example Playbook
----------------

To installing this role from ansible-galaxy

```
ansible-galaxy install laurent_kling.vault-otp
```

Including an example of how to use it with required variables :

    ---
    - hosts: clients
      vars:
      - vault_addr: https://vault:8200
      - ssh_mount_point: ssh
      - vault_ca_cert_file: vault.crt
      roles:
      - laurent_kling.vault-otp

License
-------

MIT

Author Information
------------------

[Laurent Kling](https://people.epfl.ch/laurent.kling/?lang=en) @ [EPFL - STI - IT](https://sti-it.epfl.ch/)

## Version

### V1.0.0

*Released: March 18, 2020*

- Initial release

Dependencies
------------

Fork of [vault-ssh-helper](https://github.com/jonathandalves/vault-ssh-helper)

----------------