apt_repository
=============

[![Build Status](https://travis-ci.org/systemli/ansible-role-apt_repository.svg?branch=master)](https://travis-ci.org/systemli/ansible-role-apt_repository)

Add third-party repositories in Debian and derivates and pin their packages.
Follows guide from [Debian wiki](https://wiki.debian.org/DebianRepository/UseThirdParty).

Requirements
------------

Debian 9+ or Ubuntu 18.04+. Other versions of Debian/Ubuntu might be supported as well, but aren't tested.

Role Variables
--------------

This role is intended to be used with `include_role`. The minimal necessary variables are:

```
url: https://...
key: |
  -----BEGIN PGP PUBLIC KEY BLOCK-----
  MY_ARMORED_KEY
  ...
```

Further possible variables (and their defaults)i are:

```
name: "{{ item.url|urlsplit('hostname') }}"
types: deb
suites: "{{ ansible_distribution_release }}"
components: main
packages: []
key_path: # a file path in the role `files` dir instead of `key`
```

Furthermore, it supports `preset` values. For an example see `vars/gitlab.yml`. PRs welcome!
Presets can be partially overridden.

Example Playbook
----------------

```
...
tasks:
  - name: Add gitlab-ce repo
    include_role:
      name:  systemli.apt_repository
    loop:
      - name: packages.gitlab.com
        url: https://packages.gitlab.com/gitlab/gitlab-ce/debian/
        key: "{{ gitlab_ce_key }}"
        packages:
          - gitlab-ce
...
```
or
```
...
tasks:
  - name: Add gitlab-ce repo
    include_role:
      name:  systemli.apt_repository
    loop:
      - preset: gitlab
...
```

License
-------

GPLv3

Author Information
------------------

systemli.org
