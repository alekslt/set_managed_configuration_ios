# Requirements and References

_This article describes a solution **Josef Fuchs**, Network and Security Engineer at **Pankl Racing Systems AG Austria** developed while attending the Building **Network Automation Solutions** online course._


The solution was tested in this environment:

* ansible &nbsp; 2.7.5
* python version = 3.6.7 (default, Oct 22 2018, 11:32:17) [GCC 8.2.0]
* Python module ( pip install ... )
  * textfsm &nbsp; 0.4.1
  * napalm &nbsp; 2.3.3
  * var_dump &nbsp; 1.2

* Tested with:
  * Cisco Catalyst 9300-48P &nbsp; (16.6.4a)
  * Cisco Catalyst WS-C3560CX-8XPD-S &nbsp; (15.2(4)E5)
  * Cisco Catalyst 2960
  * Cisco Catalyst 6824 &nbsp; (15.5(1)SY2)

## List of files

```bash
ansible@ubuntu:~/set_managed_configuration_ios (master)*$ tree
.
├── ansible.cfg
├── blog       # blog article as published
├── compiled
│   └── R01    # directory where template files for device R01 are stored during runtiome
│       ├── R01_0001_unmanaged_configuration.ios
│       ├── R01_0010_vlan_configuration.ios
│       ├── R01_0100_acl_emergency_access_configuration.ios
│       ├── R01_0800_client_ports_configuration.ios
│       ├── R01_9010_banner_client_ports_configuration.ios
│       ├── R01_9999_end.ios
│       └── R01_managed_client_ports.yml
├── configs
│   └── R01 # directory where the managed configuration and the differnce to running configuratio is stored
│       ├── R01__ios_banner.ios   # data generated for set_ios_banner.yml
│       ├── R01_managed_configuration.ios
│       └── R01_managed_configuration.ios.diff
├── filter_plugins
│   ├── client_intf_str.py
│   ├── ios_config_section.py     # ansible filter plugin
│   └── __pycache__
│       └── ios_config_section.cpython-36.pyc
├── group_vars
│   ├── all.yml
│   ├── ios.yml
│   ├── lab.yml
│   └── switches
│       ├── switches_vault.yml   # encrypted vars naming: vault_xxxxxx
│       └── switches.yml
├── host_vars
│   └── R01
│       ├── conf_client_ports.yml  # manual
│       ├── device_uplinks.yml     # automatically generated from datamodel
│       ├── device_vlans.yml
│       └── R01.yml
├── include
│   ├── inc_set_managed_configuration_ios.yml
│   ├── inc_template.yml
│   ├── inc_validate_directories.yml
│   └── inc_validate_directory_already_exists.yml
├── inv_develop.yml   # inventory file
├── library
│   ├── iosconfigregexp.py        # Class used by filter
│   ├── __pycache__
│   │   └── iosconfigregexp.cpython-36.pyc
│   └── test_iosconfigregexp.py   # Unittest for class
├── LICENSE
├── README.md
├── reports                      # default directory for reports (not used here)
├── set_ios_banner_motd.yml             # Playbook to set the MOTD banner
├── set_managed_configuration_ios.yml   # Playbook to push managed configuration to device
├── templates
│   ├── config_section_extractor.j2
│   ├── config_section_ios_banner_extractor.j2
│   ├── config_section_ios_banner_remover.j2
│   ├── config_section_remover.j2
│   ├── gen_managed_client_interface_list.j2
│   ├── ios
│   │   ├── config_acl_emergency_access.j2
│   │   ├── config_client_interfaces.j2
│   │   ├── config_ios_banner_motd.j2
│   │   ├── config_vlans.j2
│   │   └── ios_banner_motd.j2
│   └── textfsm
│       └── cisco_ios_show_run_interface_part.template
└── vars
    └── PLANT_A                # fabric datamodel
        ├── uplinks_db.yml
        └── vlan_db.yml
```

## Github

The full source is available at github.

<https://github.com/jifox/set_managed_configuration_ios>

## NAPALM Compatibilty

The solution relies on NAPALM (Network Automation and Programmability Abstraction Layer with Multivendor support) to fetch and replace device configuration.

A detailed description of the napalm module can be found here: <https://napalm.readthedocs.io/en/latest/index.html>

The IOS network devices must be configured to work with NAPALM. See [See this document](https://napalm.readthedocs.io/en/latest/support/ios.html) for details.


```text
! NAPALM compatible settings
archive
 path flash:archive
!
ip scp server enable
!
```

## Author:

**DI Josef Fuchs, MSc.**

Network and Security Engineer at Pankl Racing Systems AG Austria. (<https://www.pankl.com>)

Email:  <mailto:josef.fuchs (-at-) j-fuchs.at><br>
LinkedIn: <https://www.linkedin.com/pub/fuchs-josef/75/a38/16b>
