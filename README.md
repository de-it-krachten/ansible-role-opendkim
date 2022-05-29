[![CI](https://github.com/de-it-krachten/ansible-role-opendkim/workflows/CI/badge.svg?event=push)](https://github.com/de-it-krachten/ansible-role-opendkim/actions?query=workflow%3ACI)


# ansible-role-opendkim

<basic role description>


Platforms
--------------

Supported platforms

- Red Hat Enterprise Linux 7<sup>1</sup>
- Red Hat Enterprise Linux 8<sup>1</sup>
- CentOS 7
- RockyLinux 8
- AlmaLinux 8<sup>1</sup>
- Debian 10 (Buster)
- Debian 11 (Bullseye)
- Ubuntu 18.04 LTS
- Ubuntu 20.04 LTS
- Ubuntu 22.04 LTS

Note:
<sup>1</sup> : no automated testing is performed on these platforms

Role Variables
--------------
<pre><code>
# OpenDKIM private key
opendkim_private_key: /etc/opendkim/keys/dkim.private.key

# OpenDKIM public key
opendkim_public_key: /etc/opendkim/keys/dkim.public.key

# List of domain
opendkim_domains: "{{ [ opendkim_domain ] }}"

# Private key bits
opendkim_key_bits: 1024

# OS user/group
opendkim_user: opendkim
opendkim_group: opendkim

# Default settings
opendkim_settings:
  Mode: sv
  Selector: key
  ReportAddress:      '"{{ opendkim_domain }} postmaster" <postmaster@{{ opendkim_domain }}>'
  KeyTable:           '/etc/opendkim/KeyTable'
  SigningTable:       'refile:/etc/opendkim/SigningTable'
  ExternalIgnoreList: 'refile:/etc/opendkim/TrustedHosts'
  InternalHosts:      'refile:/etc/opendkim/TrustedHosts'
  Socket:             'inet:8891@localhost'
</pre></code>


Example Playbook
----------------

<pre><code>
- name: sample playbook for role 'opendkim'
  hosts: all
  vars:
    opendkim_domain: voorbeeld.nl
  tasks:
    - name: Include role 'opendkim'
      include_role:
        name: opendkim
</pre></code>
