Ansible Role: ansible_role_wincollect_agent_7
=========

Installs/upgrades/configures IBM WinCollect 7 agent.

WARNING When using XPath filters, ensure that no more than 10 filters are applied or system performance may be poor.
This role supports the following Operating Systems:

<ul>
<li>Windows Server 2016
<li>Windows Server 2019
<li>Windows Server 2022
</ul>


Requirements
------------

This role depends on `community.general`, `ansible.windows`, `community.windows` Ansible collections.

Role Variables
--------------

Available variables are listed below, along with default values where applicable (see `defaults/main.yml`):


| Variable | Required | Default | Comments |
| -------- | -------- | ------- | -------- |
| `wincollect_agent_custom_querys` | No | | Contains custom XPath queries to be performed. To enable a custom Xpath query, two variables need to be set, `path` and `query`. The `path` variable should contain the path for the query ex `Microsoft-Windows-Windows Firewall With Advanced Security/Firewall` The `query` should contain the XPath query to perform, ex `[System[(Level=4 or Level=0) and ( (EventID=2004 or EventID=2005 or EventID=2006 or EventID=2009 or EventID=2033) )]]`. Keep this below 10 querys to avoid performance issues, See example config below. |
| `wincollect_agent_filter_application` | No | | These variables contains filter settings for the relevant eventlog source. For each source two variables needs to be set, `type` and `events`. The `type` variable must be set to any of the following types: `NSAlist` - contains a list of events to monitor recommended by NSA. `Whitelist` - only send the whitelisted events to IBM QRadar. `Blacklist` - blacklist these events will prevent the agent to forwarding the events to IBM QRadar. `Nofilter` - no filtering is performed on the events in this eventlogsource. The `events` variable should contain a list of events to be added to the eventtype specified in the `type` variable. For `NSAlist` the events will be added to the `NSAlist` filters as events to monitor, any duplicate events will be removed. See example playbook below. |
| `wincollect_agent_filter_directoryservice` | No | | These variables contains filter settings for the relevant eventlog source. For each source two variables needs to be set, `type` and `events`. The `type` variable must be set to any of the following types: `NSAlist` - contains a list of events to monitor recommended by NSA. `Whitelist` - only send the whitelisted events to IBM QRadar. `Blacklist` - blacklist these events will prevent the agent to forwarding the events to IBM QRadar. `Nofilter` - no filtering is performed on the events in this eventlogsource. The `events` variable should contain a list of events to be added to the eventtype specified in the `type` variable. For `NSAlist` the events will be added to the `NSAlist` filters as events to monitor, any duplicate events will be removed. See example playbook below. |
| `wincollect_agent_filter_dns` | No | | These variables contains filter settings for the relevant eventlog source. For each source two variables needs to be set, `type` and `events`. The `type` variable must be set to any of the following types: `NSAlist` - contains a list of events to monitor recommended by NSA. `Whitelist` - only send the whitelisted events to IBM QRadar. `Blacklist` - blacklist these events will prevent the agent to forwarding the events to IBM QRadar. `Nofilter` - no filtering is performed on the events in this eventlogsource. The `events` variable should contain a list of events to be added to the eventtype specified in the `type` variable. For `NSAlist` the events will be added to the `NSAlist` filters as events to monitor, any duplicate events will be removed. See example playbook below. |
| `wincollect_agent_filter_filereplicationservice` | No | | These variables contains filter settings for the relevant eventlog source. For each source two variables needs to be set, `type` and `events`. The `type` variable must be set to any of the following types: `NSAlist` - contains a list of events to monitor recommended by NSA. `Whitelist` - only send the whitelisted events to IBM QRadar. `Blacklist` - blacklist these events will prevent the agent to forwarding the events to IBM QRadar. `Nofilter` - no filtering is performed on the events in this eventlogsource. The `events` variable should contain a list of events to be added to the eventtype specified in the `type` variable. For `NSAlist` the events will be added to the `NSAlist` filters as events to monitor, any duplicate events will be removed. See example playbook below. |
| `wincollect_agent_filter_forwardedevents` | No | | These variables contains filter settings for the relevant eventlog source. For each source two variables needs to be set, `type` and `events`. The `type` variable must be set to any of the following types: `NSAlist` - contains a list of events to monitor recommended by NSA. `Whitelist` - only send the whitelisted events to IBM QRadar. `Blacklist` - blacklist these events will prevent the agent to forwarding the events to IBM QRadar. `Nofilter` - no filtering is performed on the events in this eventlogsource. The `events` variable should contain a list of events to be added to the eventtype specified in the `type` variable. For `NSAlist` the events will be added to the `NSAlist` filters as events to monitor, any duplicate events will be removed. See example playbook below. |
| `wincollect_agent_filter_security` | No | | These variables contains filter settings for the relevant eventlog source. For each source two variables needs to be set, `type` and `events`. The `type` variable must be set to any of the following types: `NSAlist` - contains a list of events to monitor recommended by NSA. `Whitelist` - only send the whitelisted events to IBM QRadar. `Blacklist` - blacklist these events will prevent the agent to forwarding the events to IBM QRadar. `Nofilter` - no filtering is performed on the events in this eventlogsource. The `events` variable should contain a list of events to be added to the eventtype specified in the `type` variable. For `NSAlist` the events will be added to the `NSAlist` filters as events to monitor, any duplicate events will be removed. See example playbook below. |
| `wincollect_agent_filter_system` | No | | These variables contains filter settings for the relevant eventlog source. For each source two variables needs to be set, `type` and `events`. The `type` variable must be set to any of the following types: `NSAlist` - contains a list of events to monitor recommended by NSA. `Whitelist` - only send the whitelisted events to IBM QRadar. `Blacklist` - blacklist these events will prevent the agent to forwarding the events to IBM QRadar. `Nofilter` - no filtering is performed on the events in this eventlogsource. The `events` variable should contain a list of events to be added to the eventtype specified in the `type` variable. For `NSAlist` the events will be added to the `NSAlist` filters as events to monitor, any duplicate events will be removed. See example playbook below. |
| `wincollect_agent_hostname` | No | {{ ansible_hostname }} | |
| `wincollect_agent_install_dir` | No | C:\Program Files\IBM\WinCollect | |
| `wincollect_agent_install_parameters` | No | See defaults/main.yml | |
| `wincollect_agent_installer_file` | Yes | files/wincollect-7.3.0-24.x64.exe | References the path to IBM WinCollect installer ex, `files/wincollect-7.3.0-24.x64.exe`. Note that the file version in the filename, 7.3.0 in this case is used by the role to assess whether the agent is to be updated or not. |
| `wincollect_agent_log_application` | No | true | |
| `wincollect_agent_log_directory_service` | No | false | |
| `wincollect_agent_log_dns_server` | No | false | |
| `wincollect_agent_log_file_replication_service` | No | false | |
| `wincollect_agent_log_forwardedevents` | No | false | |
| `wincollect_agent_log_security` | No | true | |
| `wincollect_agent_log_system` | No | true | |
| `wincollect_agent_maxlogstoprocess` | No | 750 | |
| `wincollect_agent_minlogstoprocess` | No | 500 | |
| `wincollect_agent_profile` | No | Typical Server | |
| `wincollect_agent_tuning_profile` | No | Default (Endpoint) | |
| `wincollect_agent_version` | No | {{ wincollect_agent_installer_file | regex_search('([0-9]\\.)+[0-9]-[0-9]+') | regex_replace('-','.') }} | |
| `wincollect_syslog_status_server` | No | {{ wincollect_target_address }} | |
| `wincollect_target_address` | Yes | | References the IP or FQDN of the IBM Qradar system recieving the events. |
| `wincollect_target_port` | No | 514 | |
| `wincollect_target_protocol` | No | udp | |


Dependencies
------------

None.

Example Playbook
----------------

    - hosts: servers

      vars:
        wincollect_agent_installer_file: 'files/wincollect-7.3.0-24.x64.exe'
        wincollect_target_address: 192.168.0.1
        wincollect_target_protocol: udp
        wincollect_target_port: 514
        wincollect_agent_log_security: true
        wincollect_agent_log_application: true
        wincollect_agent_log_system: true
        wincollect_agent_log_directory_service: false
        wincollect_agent_log_file_replication_service: false
        wincollect_agent_log_forwardedevents: false
        wincollect_agent_log_dns_server: false
        wincollect_agent_filter_security:
          type: nsalist
          events: [4964,4947,4950,4954,4964,5031,5155]
        wincollect_agent_filter_system:
          type: nsalist
          events: [104,1127,5025]
        wincollect_agent_filter_application:
          type: nsalist
          events: [2,104]
        wincollect_agent_filter_dns:
          type: nofilter
          events: []
        wincollect_agent_filter_filereplicationservice:
          type: nofilter
          events: []
        wincollect_agent_filter_directoryservice:
          type: nofilter
          events: []
        wincollect_agent_filter_forwardedevents:
          type: nofilter
          events: []
        wincollect_agent_custom_querys:
          - path: "Microsoft-Windows-Windows Firewall With Advanced Security/Firewall"
            query: "*[System[(Level=4 or Level=0) and ( (EventID=2004 or EventID=2005 or EventID=2006 or EventID=2009 or EventID=2033) )]]"


      roles:
        - ansible_role_wincollect_agent_7

License
-------

MIT

Author Information
------------------

Mattias Jonsson



