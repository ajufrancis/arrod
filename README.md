Ansible plays to deploy OpenStack with Neutron(Quantum) on RHEL 6.4+ or Fedora 19+

**Notes**  
This work was initialy based from [ansible-openstack](https://github.com/ansible/ansible-redhat-openstack).  
Because of different deployment needs, for multi nodes approach, it has been almost entirely revisited.  
Ultimately they will be merged back together.  
Storage and HA are not supported yet but working on it.

**Assumptions** 
  1. Using 2 physical networks
  2. A consistent network interface naming and configuration accross the environment.  
     For instance:
     * eth1 is always external network 
     * eth2 is always internal/data network 

**How To**  
  1.  Requirements
      * Ansible 1.2+ installed on a system having ssh access to all your OpenStack target host(s)
      * Targetted hosts must have
        * RHEL6.4+ or Fedora 19+ installed
        * OpenStack repos
          * RHEL: [RDO repo](repos.fedorapeople.org/repos/openstack/) or RHOS 3.0 channel
          * Fedora repo
      * If using RHOS, also consult README-RHOS30
  2. Create a `hosts` inventory file, use hosts-examples directory for templates:
     * All in one:  
       1 x Controller/Network/Compute node 
     * Controller and network service:  
       N x controller(s), N x compute node(s)
     * All separate:  
       N x controller(s), N x network node(s), N x compute node(s)
  3. Edit group_vars values:
     * Passwords and secrets keys
     * Neutron
       * Set `quantum_external_interface` and `quantum_internal_interface` keys, see Assumptions above
       * VLANs
         * Set `provider_network_type: vlan`
         * Set `splinters: True` if [needed](https://access.redhat.com/site/articles/289823)
       * GRE tunnels
         * Set `provider_network_type: gre`
  4. Deploy
     1. Make sure you have ssh access to all hosts  
        For instance have an ssh key installed on all hosts and use ssh-agent
     2. Run Ansible using inventory file created above  
        `# ansible-playbooks -i hosts site.yml`
     3. If it fails, address issue according to the message and re-run Ansible: wash, rince and repeat!

