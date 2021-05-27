Ansible Role: Static Host Configuration
=======================================

An Ansible role to set static information about self and other hosts in
`/etc/hosts`.


Table of Contents
-----------------

<!-- toc -->

- [Requirements](#requirements)
- [Role Variables](#role-variables)
- [Dependencies](#dependencies)
- [Example Playbook](#example-playbook)
  * [Top-Level Playbook](#top-level-playbook)
  * [Role Dependency](#role-dependency)
- [License](#license)
- [Author Information](#author-information)

<!-- tocstop -->


Requirements
------------

- Ansible 2.10+


Role Variables
--------------

Self describing information:

```yml
system_hostname: '{{ ansible_hostname }}'
system_domain: example.com
```

Whether to manage self with a local IP entry:

```yml
system_hosts_manage_self: yes
```

This would create an entry like that:

```
127.0.1.1 {{ system_hostname }}
```

Additional hosts for `/etc/hosts`, grouped in sections:

```yml
system_hosts_sections:

  - name: database servers
    entries:

      - name: mars.example.com
        address: 192.168.1.1
        aliases: [mars]

      - name: venus.example.com
        address: 192.168.1.2
        aliases: [venus]

  - name: web servers
    entries:

      - name: jupiter.example.com
        address: 192.168.2.1
        aliases: [jupiter]

      - name: saturn.example.com
        address: 192.168.2.2
        aliases: [saturn]
```

Dependencies
------------

None.


Example Playbook
----------------

Add to `requirements.yml`:

```yml
---

- src: idiv-biodiversity.hosts

...
```

Download:

```console
$ ansible-galaxy install -r requirements.yml
```

### Top-Level Playbook

Write a top-level playbook:

```yml
---

- name: head server
  hosts: head

  roles:
    - role: idiv-biodiversity.hosts
      tags:
        - hosts

...
```

### Role Dependency

Define the role dependency in `meta/main.yml`:

```yml
---

dependencies:

  - role: idiv-biodiversity.hosts
    tags:
      - hosts

...
```


License
-------

MIT


Author Information
------------------

This role was created in 2021 by [Christian Krause][author] aka [wookietreiber
at GitHub][wookietreiber], HPC cluster systems administrator at the [German
Centre for Integrative Biodiversity Research (iDiv)][idiv].


[author]: https://www.idiv.de/en/groups_and_people/employees/details/61.html
[idiv]: https://www.idiv.de/
[wookietreiber]: https://github.com/wookietreiber
