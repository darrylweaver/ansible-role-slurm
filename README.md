SLURM cluster Role 
=======================

Install SLURM cluster [1]. This role is for deploying Slurm on controller and worker nodes.

Dependencies
------------

This role requires an NTP client to sync the time on all nodes.  Many suitable clients can be used.

Role Variables
--------------

The variables that can be passed to this role and a brief description about them are as follows.

	# SLURM version to install (in case of RH systems)
	slurm_version: 16.05.8
	# Type of node to install: controller or worker
	slurm_type_of_node: controller
	# Name of the SLURM controller
	slurm_controller_name: slurmserver
	# IP address of the SLURM controller
	slurm_controller_ip: 127.0.0.1
	# Prefix to set to the SLURM working nodes
	slurm_vnode_prefix: vnode-
	# List of IPs of the Worker Nodes (WNs)
	slurm_worker_ips: []
	# List of the name of the WNs (use short hostname)
	slurm_worker_nodenames: []
	# Number of CPUs of the WNs
	slurm_worker_cpus: 1
        # Partition Name
        slurm_partition_name: debug

Example Playbook
----------------

This an example of how to install a SLURM cluster:

    - hosts: controller
      roles:
      - { role: 'darrylweaver.slurm', slurm_type_of_node: 'controller', slurm_server_ip: '{{ansible_default_ipv4}}', slurm_worker_nodenames: "{{ groups['wns']|map('extract', hostvars, 'ansible_hostname')|list }}" }

    - hosts: workers
      roles:
      - { role: 'darrylweaver.slurm', slurm_type_of_node: 'worker', slurm_server_ip: "{{hostvars['server']['ansible_default_ipv4']}}" }

License
-------

Apache Licence v2 [2]

[1] http://slurm.schedmd.com/

[2] http://www.apache.org/licenses/LICENSE-2.0

Credits
-------

Originally forked from indigo-dc/slurm role.
Modified by Darryl Weaver.

