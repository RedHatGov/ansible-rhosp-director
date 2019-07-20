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
  tags: install
  vars:
    domain: "example.com"
    dns_server_public: 1.1.1.1
    director_hostname: director #Short hostname
    director_ssh_user: root
    director_ssh_pwd: redhat
    director_public_ip: "192.168.0.4"
    director_base_img: rhel-guest-image-7.qcow2 #Name of base image in /var/lib/libvirt/images on KVM hypervisor
    director_os_disk_name: "{{ director_hostname }}"
    director_nics:
      - name: eth0
        bootproto: static
        onboot: yes
        ip: "{{ director_public_ip }}"
        prefix: "24"
        gateway: "192.168.0.1"
        dns_server: "{{ dns_server_public }}"
        config: "--type bridge --source br1 --model virtio"
    director_repos:
      - rhel-7-server-rpms
      - rhel-7-server-extras-rpms
      - rhel-7-server-optional-rpms
    director_packages:
      - ipa-server
      - ipa-server-dns
    director_realm: "{{ domain | upper }}"
    director_dm_pwd: "Redhat1993"
    director_admin_pwd: "Redhat1993"
    director_forward_ip: "{{ dns_server_public }}"
    director_reverse_zone: "168.192.in-addr.arpa."
    director_users:
       - username: operator
         password: redhat1234
         display_name: "Operator"
         first_name: Oper
         last_name: Ator
         email: "operator@redhat.com"
         phone: "+18887334281"
         title: "Systems Administrator"
    director_dns_records:
       - hostname: router
         record_type: A
         ip_address: 192.168.0.1
         reverse_record: 1.0
       - hostname: switch
         record_type: A
         ip_address: 192.168.0.2
         reverse_record: 2.0
       - hostname: kvm
         record_type: A
         ip_address: 192.168.0.3
         reverse_record: 3.0
  tasks:
    - name: Install director
      include_role:
        name: director
      tags: install

    - name: Configure director
      include_role:
        name: director
        tasks_from: post_config
      tags: post-config
```

License
-------

GPLv3

Author Information
------------------

[Red Hat North American Public Sector Solution Architects](https://redhatgov.io)
