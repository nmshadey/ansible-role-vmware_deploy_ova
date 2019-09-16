vmware_deploy_ova
=========

This role can be used to import an OVA/OVF template to a target VMware vCenter Server or VMware ESXi host.

Requirements
------------

- python >= 2.6
- PyVmomi

Role Variables
--------------

  ### Default variables that have been defined in defaults/main.yml

  Set where the OVA file should be sourced.

  Current options are '**local**' and '**http**'.

  If **local** is set, then the **ova_file** will be sourced from the defined **ova_path**.

  If **http** is set, then the **ova_file** will be downloaded to the defined **ova_path**.

  For **http**, the **ova_url** also needs to be set.
  ```
  ova_source: "local"
  ```

  Whether or not certificate verification should be enabled against the target host the OVA is being imported to.
  ```
  ova_validate_certs: no
  ```

  Whether or not duplicates of the same name are allowed.
  ```
  ova_allow_duplicates: no
  ```

  Whether or not the appliance should be powered on once the OVA import process has completed.
  ```
  ova_power_on_after_deploy: yes
  ```

  Whether or not the module should wait for the IP address to become available in vCenter after powering on the OVA.
  ```
  ova_wait_for_ip_address: no
  ```

  The default disk format to use for the imported OVA's disks.
  ```
  ova_deployment_disk_type: thin
  ```

  Hardware Configuration
  ```
  ova_hardware_hotadd_cpu_enabled: true
  ova_hardware_hotadd_mem_enabled: true
  ```

  ### The following parameters need to be provided, as extra vars, group_vars or host_vars:

  #### OVA Deployment Variables

  Set the OVA deployment variables.
  ```
  ova_deployment_hostname: "vcenter/esxi hostname"
  ova_deployment_username: "vcenter/esxi username"
  ova_deployment_password: "vcenter/esxi password"
  ```

  Set the target datastore. Datastore clusters are not supported by the module.
  ```
  ova_deployment_datastore: "datastore"
  ```

  The following are only required when deploying to vCenter Server. If folder is not defined then the appliance will deploy to the default folder.
  ```
  ova_deployment_datacenter: "vcenter datacenter"
  ova_deployment_cluster: "vcenter cluster"
  ova_deployment_folder: "vcenter folder"
  ```

  ### The following mandatory global variables need to be set:

  #### OVA Configuration

  Set the OVA file name.
  ```
  ova_file: "ova_file.ova"
  ```

  Set the local path to the OVA file (do not use a leading /).
  ```
  ova_path: "/path/to/ova_file"
  ```

  ### The following optional global variables can be set:

  #### OVA Download Configuration

  Set the URL to the OVA file if source is set to 'http' (do not use a leading /).
  ```
  ova_url: "http[s]://example.com/ovas"
  ```

  ### The following mandatory variables need to be set in roles that use this role:

  #### OVA Properties

  A key:value pair for the network property.
  ```
  ova_networks:
    "key":"value"
  ```
  Example:
  ```
  ova_networks:
    "Network 1": "label"
  ```

  The OVA Properties as a set of dictionary key: value pairs.
  ```
  ova_properties:
    "key": "value"
    "key": "value"
  ```
  Example:
  ```
  ova_properties:
    "guestinfo.cis.appliance.net.addr.family": "ipv4"
    "guestinfo.cis.appliance.net.mode": "static"
  ```

  ### The following optional variables can be set in roles that use this role:

  A string containing the deployment option.
  ```
  ova_deployment_option: "option"
  ```

  #### OVA vApp Properties

  The vApp Properties as list of dictionaries with userConfigurable set to true.
  ```
  vapp_properties: 
    - list of dict1
    - list of dict2
  ```
  Example:
  ```
  vapp_properties:
    - id: guestinfo.cis.deployment.node.type
      type: string
      value: "embedded"
      userConfigurable: true
    - id: guestinfo.cis.appliance.ssh.enabled
      type: boolean
      value: "true"
      userConfigurable: true
  ```
  #### Hardware Configuration

  Set the number of CPU sockets.
  ```
  ova_hardware_num_cpus: 2
  ```

  Set the amount of memory in GB.
  ```
  ova_hardware_mem_gb: 6
  ```

  Set the disks to add.
  ```
  ova_hardware_disks:
    - size_gb: 2
      type: thin # Hard Disk 1
    - size_gb: 4
      type: thin # Hard Disk 2
  ```
  
  Set the networks to add.
  ```
  ova_hardware_networks:
    - name: "network label"  # nic 1
    - name: "network label"  # nic 2
  ```
Example Playbook
----------------

This role is designed to be used as a dependency for parent roles.