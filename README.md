Role Name
=========

Install and configure haproxy

Requirements
------------

Works on only CentOS 6.xx, and haproxy 1.5.2.

Role Variables
--------------

There are the following 4 main sections of variables
  - global
  - defaults
  - frontends
  - backends

defaults/main.yml keeps global/defaults portions, frontends/backends variables need to be defined in playbook.

Dependencies
------------

No dependency needed

Example Playbook
----------------

Here is the sample playbook you can refer.

```yaml
- hosts: haproxy
  sudo: yes
  gather_facts: yes
  roles:
  - role: haproxy
    haproxy_version: 1.5
    haproxy_frontends:
      - name: 'frontend-http'
        ip: '192.168.33.13'
        port: '80'
        maxconn: '1000'
        acl:
          - name: 'url_blog'
            condition: 'path_beg /blog'
        use_backend:
          - name: 'backend-blog'
            condition: 'if url_blog'
        default_backend: 'backend-http'
    haproxy_backends:
      - name: 'backend-http'
        balance: 'roundrobin'
        servers:
          - name: 'web01'
            ip: '192.168.33.15'
            port: '80'
          - name: 'web02'
            ip: '192.168.33.16'
            port: '80'
      - name: 'backend-blog'
        balance: 'roundrobin'
        servers:
          - name: 'web01'
            ip: '192.168.33.15'
            port: '80'
          - name: 'web03'
            ip: '192.168.33.14'
            port: '80'
```


Example flow
------------
1. Create ansible environment, referring to http://docs.ansible.com/playbooks_best_practices.html

2. cd "ansible environment root directory"/roles && git clone https://github.com/jshima7/ansible-role-haproxy -b master haproxy

3. Run ansible-playbook commands

```yaml
Apply all tasks
  ansible-playbook -i [INVENTORY FILE] [PLAYBOOK YAML]

Apply only 'check-os-version' task
  ansible-playbook -i [INVENTORY FILE] [PLAYBOOK YAML] -t check-os-version

Apply only 'common-packages' task
  ansible-playbook -i [INVENTORY FILE] [PLAYBOOK YAML] -t common-packages

Apply only 'haproxy' task
  ansible-playbook -i [INVENTORY FILE] [PLAYBOOK YAML] -t haproxy
```



Author Information
------------------

Junichiro Shimamura
