[![CI](https://github.com/de-it-krachten/ansible-role-opendkim/workflows/CI/badge.svg?event=push)](https://github.com/de-it-krachten/ansible-role-opendkim/actions?query=workflow%3ACI)


# ansible-role-opendkim

Install & manages OpenDKIM 



## Dependencies

#### Roles
None

#### Collections
- community.crypto

## Platforms

Supported platforms

- Red Hat Enterprise Linux 7<sup>1</sup>
- Red Hat Enterprise Linux 8<sup>1</sup>
- Red Hat Enterprise Linux 9<sup>1</sup>
- CentOS 7
- RockyLinux 8
- RockyLinux 9
- OracleLinux 8
- OracleLinux 9
- AlmaLinux 8
- AlmaLinux 9
- Debian 11 (Bullseye)
- Debian 12 (Bookworm)
- Ubuntu 20.04 LTS
- Ubuntu 22.04 LTS
- Ubuntu 24.04 LTS
- Fedora 39
- Fedora 40

Note:
<sup>1</sup> : no automated testing is performed on these platforms

## Role Variables
### defaults/main.yml
<pre><code>
# OpenDKIM private key
opendkim_private_key: /etc/opendkim/keys/dkim.private.key

# OpenDKIM public key
opendkim_public_key: /etc/opendkim/keys/dkim.public.key

# List of domain
opendkim_domains: "{{ [opendkim_domain] }}"

# Private key bits
opendkim_key_bits: 1024

# OS user/group
opendkim_user: opendkim
opendkim_group: opendkim

# Default settings
opendkim_settings:
  Mode: sv
  Selector: key
  ReportAddress: '"{{ opendkim_domain }} postmaster" <postmaster@{{ opendkim_domain }}>'
  KeyTable: '/etc/opendkim/KeyTable'
  SigningTable: 'refile:/etc/opendkim/SigningTable'
  ExternalIgnoreList: 'refile:/etc/opendkim/TrustedHosts'
  InternalHosts: 'refile:/etc/opendkim/TrustedHosts'
  Socket: 'inet:8891@localhost'

opendkim_postfix_settings:
  milter_default_action: accept
  milter_protocol: '6'
  smtpd_milters: 'inet:127.0.0.1:8891'
  non_smtpd_milters: $smtpd_milters
</pre></code>

### defaults/family-Debian.yml
<pre><code>
opendkim_packages:
  - opendkim
  - opendkim-tools
</pre></code>

### defaults/family-RedHat-7.yml
<pre><code>
opendkim_packages:
  - opendkim
</pre></code>

### defaults/family-RedHat.yml
<pre><code>
opendkim_packages:
  - opendkim
  - opendkim-tools
</pre></code>

### defaults/family-Suse.yml
<pre><code>
opendkim_packages:
  - opendkim
  - opendkim-tools
</pre></code>




## Example Playbook
### molecule/default/converge.yml
<pre><code>
- name: sample playbook for role 'opendkim'
  hosts: all
  become: 'yes'
  vars:
    postfix_ipv6: false
    postfix_domain: example.com
    postfix_fqdn: host.example.com
    postfix_ssl_key: '{{ openssl_server_key }}'
    postfix_ssl_chain: '{{ openssl_server_crt }}'
    opendkim_domain: example.com
  roles:
    - deitkrachten.cron
    - deitkrachten.openssl
    - deitkrachten.postfix
  tasks:
    - name: Include role 'opendkim'
      ansible.builtin.include_role:
        name: opendkim
</pre></code>
