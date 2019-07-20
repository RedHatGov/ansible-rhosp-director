director
=========

This role provisions, installs, and configures Red Hat OpenStack Platform Director

Requirements
------------

- Expects a working RHEL 7 system to target
- Red Hat Network account with a Red Hat OpenStack Platform subscription available

Role Variables
--------------

| Variable        | Required | Default  | Description                                                                                                                                                                                                                                     |
| --------------- | -------- | -------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `domain` | :x:      | ```example.com``` | The domain for the environment |
| `dns_server_local` | :heavy_check_mark:      |  | The default local DNS server to use |
| `ntp_server` | :x:      | ```0.pool.ntp.org``` | The default NTP server to use |
| `versions` | :x:      | ```see defaults/main.yml``` | A dictionary of Red Hat software versions |
| `networks` | :x:      | ```see defaults/main.yml``` | A dictionary of local network vlans to use for the RHOSP deployment |
| `director_hostname` | :x:      | ```director``` | The short hostname for director |
| `director_external_ip` | :heavy_check_mark:      |  | The IP for director on the external OpenStack network |
| `director_stack_user_pwd` | :heavy_check_mark:      |  | The default password to use for the stack user on director |
| `director_repos` | :x:      | ```see defaults/main.yml``` | Dictionary of Repos to enable for director |
| `director_packages` | :x:      | ```see defaults/main.yml``` | Dictionary of Packages to create for director |
| `director_optional_docker_services` | :x:      | ```see defaults/main.yml``` | Dictionary of optional services that will be deployed in overcloud |
| `director_upstream_registry` | :x:      | ```registry.redhat.io``` | The fqdn of the registry to use for upstream RHOSP containers |
| `director_ceph_enabled` | :x:      | ```true``` | Boolean for whether Ceph will be deployed in overcloud |
| `director_ceph_namespace` | :x:      | ```"{{ director_upstream_registry }}/rhceph"``` | The namespace for Ceph containers |
| `director_ceph_image` | :x:      | ```rhceph-3-rhel7``` | Name for Ceph container image |
| `director_ceph_tag` | :x:      | ```latest``` | Tag to use for Ceph container image |
| `director_ceph_containerized` | :x:      | ```true``` | Boolean for whether Ceph will be deployed containerized |
| `director_cloud_domain` | :x:      | ```"{{ domain }}"``` | Domain used for RHOSP deployment |
| `director_ntp_servers` | :x:      | ```"{{ ntp_server }}"``` | NTP server used for RHOSP deployment |
| `director_provisioning_interface` | :x:      | ```eth0``` | Interface name on director for provisioning network |
| `director_provisioning_interface_mtu` | :x:      | ```1500``` | MTU for provisioning interface on director |
| `director_provisioning_ip` | :x:      | ```192.168.2.5/24``` | IP address (in CIDR notation) for provisioning network |
| `director_provisioning_network_cidr` | :x:      | ```"{{ network.provisioning.cidr }}"``` | CIDR for provisioning network |
| `director_provisioning_network_gateway` | :x:      | ```"{{ network.provisioning.gateway }}"``` | Gateway for provisioning network |
| `director_admin_apis_vip` | :x:      | ```"{{ director_provisioning_ip | ipaddr('address') }}"``` | IP address for admin APIs |
| `director_inspection_dhcp_start` | :x:      | ```"{{ network.provisioning.introspectionDhcpStart }}"``` | Start address for introspection DHCP pool on provisioning network |
| `director_inspection_dhcp_end` | :x:      | ```"{{ network.provisioning.introspectionDhcpEnd }}"``` | End address for introspection DHCP pool on provisioning network |
| `director_deploy_dhcp_start` | :x:      | ```"{{ network.provisioning.deployDhcpStart }}"``` | Start address for deployment DHCP pool on provisioning network |
| `director_deploy_dhcp_end` | :x:      | ```"{{ network.provisioning.deployDhcpEnd }}"``` | End address for deployment DHCP pool on provisioning network |
| `director_clean_nodes` | :x:      | ```true``` | Boolean for enabling clean_nodes on director |
| `director_admin_password` | :x:      | ```p@ssw0rd``` | Password for admin user on director |

Dependencies
------------

None

Example Playbook
----------------

```yaml
---
- hosts: director
  tasks:
  - name: Install Director
    include_role:
      name: RedHatGov.director

  - name: Configure Director
    include_role:
      name: RedHatGov.director
      tasks_from: post_config
```

License
-------

GPLv3

Author Information
------------------

[Red Hat North American Public Sector Solution Architects](https://redhatgov.io)
