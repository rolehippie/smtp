# smtp

[![Source Code](https://img.shields.io/badge/github-source%20code-blue?logo=github&logoColor=white)](https://github.com/rolehippie/smtp) [![General Workflow](https://github.com/rolehippie/smtp/actions/workflows/general.yml/badge.svg)](https://github.com/rolehippie/smtp/actions/workflows/general.yml) [![Readme Workflow](https://github.com/rolehippie/smtp/actions/workflows/readme.yml/badge.svg)](https://github.com/rolehippie/smtp/actions/workflows/readme.yml) [![Galaxy Workflow](https://github.com/rolehippie/smtp/actions/workflows/galaxy.yml/badge.svg)](https://github.com/rolehippie/smtp/actions/workflows/galaxy.yml) [![License: Apache-2.0](https://img.shields.io/github/license/rolehippie/smtp)](https://github.com/rolehippie/smtp/blob/master/LICENSE) [![Ansible Role](https://img.shields.io/ansible/role/55298)](https://galaxy.ansible.com/rolehippie/smtp)

Ansible role to install Postfix as pure SMTP server.

## Sponsor

Building and improving this Ansible role have been sponsored by my current and previous employers like **[Cloudpunks GmbH](https://cloudpunks.de)** and **[Proact Deutschland GmbH](https://www.proact.eu)**.

## Table of content

- [Default Variables](#default-variables)
  - [smtp_default_aliases](#smtp_default_aliases)
  - [smtp_default_destinations](#smtp_default_destinations)
  - [smtp_default_master](#smtp_default_master)
  - [smtp_default_networks](#smtp_default_networks)
  - [smtp_default_relayhosts](#smtp_default_relayhosts)
  - [smtp_default_services](#smtp_default_services)
  - [smtp_dynamic_maps](#smtp_dynamic_maps)
  - [smtp_extra_aliases](#smtp_extra_aliases)
  - [smtp_extra_destinations](#smtp_extra_destinations)
  - [smtp_extra_master](#smtp_extra_master)
  - [smtp_extra_networks](#smtp_extra_networks)
  - [smtp_extra_relayhosts](#smtp_extra_relayhosts)
  - [smtp_extra_services](#smtp_extra_services)
  - [smtp_hostname](#smtp_hostname)
  - [smtp_match_destinations](#smtp_match_destinations)
  - [smtp_myorigin](#smtp_myorigin)
  - [smtp_relay_restrictions](#smtp_relay_restrictions)
  - [smtp_tls_ca_path](#smtp_tls_ca_path)
  - [smtp_tls_cert_file](#smtp_tls_cert_file)
  - [smtp_tls_key_file](#smtp_tls_key_file)
  - [smtp_tls_security_level](#smtp_tls_security_level)
- [Discovered Tags](#discovered-tags)
- [Dependencies](#dependencies)
- [License](#license)
- [Author](#author)

---

## Default Variables

### smtp_default_aliases

#### Default value

```YAML
smtp_default_aliases:
  - alias: mailer-daemon
    recipient: postmaster
  - alias: postmaster
    recipient: root
  - alias: nobody
    recipient: root
  - alias: hostmaster
    recipient: root
  - alias: usenet
    recipient: root
  - alias: news
    recipient: root
  - alias: webmaster
    recipient: root
  - alias: www
    recipient: root
  - alias: ftp
    recipient: root
  - alias: abuse
    recipient: root
  - alias: noc
    recipient: root
  - alias: security
    recipient: root
```

#### Example usage

```YAML
smtp_default_aliases:
  - alias: root
    recipient: user1@example.com
  - alias: postmaster
    recipients:
      - user1@example.com
      - user2@example.com
      - user3@example.com
```

### smtp_default_destinations

List of default postfix destinations

#### Default value

```YAML
smtp_default_destinations:
  - $myhostname
  - localhost.$mydomain
  - localhost
  - pcre:/etc/postfix/destinations.cf
```

### smtp_default_master

List of default postfix services

### smtp_default_networks

List of default postfix networks

#### Default value

```YAML
smtp_default_networks:
  - 127.0.0.0/8
```

### smtp_default_relayhosts

List of default postfix relayhosts

#### Default value

```YAML
smtp_default_relayhosts: []
```

### smtp_default_services

#### Default value

```YAML
smtp_default_services:
  - service: smtp
    weight: 5
    type: inet
    private: n
    chroot: y
    command: smtpd
  - service: pickup
    weight: 10
    type: unix
    private: n
    chroot: y
    wakeup: 60
    maxproc: 1
    command: pickup
  - service: cleanup
    weight: 15
    type: unix
    private: n
    chroot: y
    maxproc: 0
    command: cleanup
  - service: qmgr
    weight: 20
    type: unix
    private: n
    chroot: n
    wakeup: 300
    maxproc: 1
    command: qmgr
  - service: tlsmgr
    weight: 25
    type: unix
    chroot: y
    wakeup: 1000?
    maxproc: 1
    command: tlsmgr
  - service: rewrite
    weight: 25
    type: unix
    chroot: y
    command: trivial-rewrite
  - service: bounce
    weight: 30
    type: unix
    chroot: y
    maxproc: 0
    command: bounce
  - service: defer
    weight: 35
    type: unix
    chroot: y
    maxproc: 0
    command: bounce
  - service: trace
    weight: 40
    type: unix
    chroot: y
    maxproc: 0
    command: bounce
  - service: verify
    weight: 45
    type: unix
    chroot: y
    maxproc: 1
    command: verify
  - service: flush
    weight: 50
    type: unix
    private: n
    chroot: y
    wakeup: 1000?
    maxproc: 0
    command: flush
  - service: proxymap
    weight: 55
    type: unix
    chroot: n
    command: proxymap
  - service: proxywrite
    weight: 60
    type: unix
    chroot: n
    maxproc: 1
    command: proxymap
  - service: smtp
    weight: 65
    type: unix
    chroot: y
    command: smtp
  - service: relay
    weight: 70
    type: unix
    chroot: y
    command: smtp
    args:
      - -o syslog_name=postfix/$service_name
  - service: showq
    weight: 75
    type: unix
    private: n
    chroot: y
    command: showq
  - service: error
    weight: 80
    type: unix
    chroot: y
    command: error
  - service: retry
    weight: 85
    type: unix
    chroot: y
    command: error
  - service: discard
    weight: 90
    type: unix
    chroot: y
    command: discard
  - service: local
    weight: 95
    type: unix
    unpriv: n
    chroot: n
    command: local
  - service: virtual
    weight: 100
    type: unix
    unpriv: n
    chroot: n
    command: virtual
  - service: lmtp
    weight: 105
    type: unix
    chroot: y
    command: lmtp
  - service: anvil
    weight: 110
    type: unix
    chroot: y
    maxproc: 1
    command: anvil
  - service: scache
    weight: 115
    type: unix
    chroot: y
    maxproc: 1
    command: scache
  - service: postlog
    weight: 120
    type: unix-dgram
    private: n
    chroot: n
    maxproc: 1
    command: postlogd
    enabled: "{{ True if ansible_distribution_version is version('20.04', '>=') else\
      \ False }}"
```

### smtp_dynamic_maps

List of dynamic map definitions

#### Default value

```YAML
smtp_dynamic_maps: []
```

#### Example usage

```YAML
smtp_dynamic_maps:
  - type: sqlite
    soname: smtp-sqlite.so.1.0.1
    dict: dict_sqlite_open
  - type: example
    soname: smtp-example.so.1.0.0
    dict: dict_example_open
    mkmap: example
```

### smtp_extra_aliases

#### Default value

```YAML
smtp_extra_aliases: []
```

#### Example usage

```YAML
smtp_extra_aliases:
  - alias: root
    recipient: user1@example.com
  - alias: postmaster
    recipients:
      - user1@example.com
      - user2@example.com
      - user3@example.com
```

### smtp_extra_destinations

List of extra postfix destinations

#### Default value

```YAML
smtp_extra_destinations: []
```

### smtp_extra_master

List of extra postfix services

### smtp_extra_networks

List of extra postfix networks

#### Default value

```YAML
smtp_extra_networks: []
```

### smtp_extra_relayhosts

List of extra mail aliases

#### Default value

```YAML
smtp_extra_relayhosts: []
```

### smtp_extra_services

#### Default value

```YAML
smtp_extra_services: []
```

### smtp_hostname

Hostname written to postfix config

#### Default value

```YAML
smtp_hostname: '{{ ansible_fqdn }}'
```

### smtp_match_destinations

List of regexp matching destinations

#### Default value

```YAML
smtp_match_destinations: []
```

#### Example usage

```YAML
smtp_match_destinations:
  - /^.*\.example\.com$/
```

### smtp_myorigin

Origin used by postfix

#### Default value

```YAML
smtp_myorigin: /etc/mailname
```

### smtp_relay_restrictions

List of relay restrictions

#### Default value

```YAML
smtp_relay_restrictions:
  - permit_mynetworks
  - permit_sasl_authenticated
  - defer_unauth_destination
```

### smtp_tls_ca_path

Path to directory containing CAs

#### Default value

```YAML
smtp_tls_ca_path: /etc/ssl/certs
```

### smtp_tls_cert_file

Path to TLS cert

#### Default value

```YAML
smtp_tls_cert_file: /etc/ssl/certs/ssl-cert-snakeoil.pem
```

### smtp_tls_key_file

Path to TLS key

#### Default value

```YAML
smtp_tls_key_file: /etc/ssl/private/ssl-cert-snakeoil.key
```

### smtp_tls_security_level

Security level for TLS

#### Default value

```YAML
smtp_tls_security_level: may
```

## Discovered Tags

**_smtp_**


## Dependencies

- None

## License

Apache-2.0

## Author

[Thomas Boerger](https://github.com/tboerger)
