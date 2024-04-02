# Ansible Role: wincollect_agent_7

This role installs, upgrades, and configures IBM WinCollect 7 agent on Windows versions (2016, 2019, 2022).

## Requirements

This role requires the `ansible.windows` and `community.windows` collections. If these are not already installed, you can acquire them using the following command:

```shell
ansible-galaxy collection install ansible.windows community.windows

```

Role Variables
--------------

Available variables are listed below, along with default values where applicable (see `defaults/main.yml`):

## Role Variables

Variables enable customization of the IBM WinCollect agent's installation, configuration, and behavior. Below is a comprehensive list of variables you can configure:

#### **Warning**
When using XPath filters, ensure that no more than 10 filters are applied or system performance may be impacted.

### General Configuration

These variables define the setup and operational parameters for the WinCollect agent, including its installation path, connectivity details with IBM QRadar, and other settings.

| Variable                              | Required | Default | Description |
|---------------------------------------|----------|---------|-------------|
| `wincollect_agent_7_hostname`         | No       | `{{ ansible_hostname }}` | Hostname used by the IBM WinCollect 7 agent, defaulting to the Ansible-managed host's hostname. |
| `wincollect_agent_7_install_dir`      | No       | `C:\Program Files\IBM\WinCollect` | Specifies the directory where the WinCollect agent will be installed. |
| `wincollect_agent_7_installer_file`   | Yes      |  | URL or local path to the WinCollect installer file. Make sure to keep the name of installer file since version is detected based on the name. |
| `wincollect_agent_7_arch`             | No      | Auto-detected from `wincollect_agent_7_installer_file` | IBM WinCollect 7 agent architecture, valid options are x86 or x64. |
| `wincollect_agent_7_install_parameters` | No     | See `defaults/main.yml` | Provides parameters for the IBM WinCollect 7 agent installation, configuring silent installation, status server, log source auto-creation, and other settings. |
| `wincollect_agent_7_version`          | No       | Auto-detected from `wincollect_agent_7_installer_file` | The version of the IBM WinCollect 7 agent, determined from the installer file name. |
| `wincollect_agent_7_syslog_status_server`     | No       | `{{ wincollect_agent_7_target_address }}` | The address of the syslog status server, defaulting to the IBM QRadar system address. |
| `wincollect_agent_7_target_address`           | Yes      |  | The IP address or FQDN of the IBM QRadar system to receive events. |
| `wincollect_agent_7_target_port`              | No       | `514` | The port on which the IBM QRadar system listens for incoming events. |
| `wincollect_agent_7_target_protocol`          | No       | `udp` | The protocol used for sending events to IBM QRadar (`udp` or `tcp`). |
| `wincollect_agent_7_sha256sum`          | No       | SHA256 checksums are provided within `defaults/main.yml`. | Map of IBM WinCollect 7 agent installer versions to their SHA256 checksums for integrity verification. |

### Performance Tuning

Performance tuning variables allow you to optimize the operation of the WinCollect agent.

| Variable                                   | Required | Default           | Description |
|--------------------------------------------|----------|-------------------|-------------|
| `wincollect_agent_7_min_logs_to_process`   | No       | `500`             | Minimum threshold for the number of logs to process per cycle. |
| `wincollect_agent_7_max_logs_to_process`   | No       | `750`             | Maximum number of logs the agent will process in a given cycle. |
| `wincollect_agent_7_profile`               | No       | `Typical Server`  | Predefined performance profile for the agent. Adjust this setting based on the operational role of the server for optimal performance. |
| `wincollect_agent_7_tuning_profile`        | No       | `Default (Endpoint)` | Specifies the tuning profile for optimal performance based on the deployment environment. |

### Log Management

| Variable                                   | Required | Default | Description |
|--------------------------------------------|----------|---------|-------------|
| `wincollect_agent_7_log_application`       | No       | `true`  | Enables logging for the Application event log. |
| `wincollect_agent_7_log_directory_service` | No       | `false` | Enables logging for the Directory Service event log. |
| `wincollect_agent_7_log_dns_server`        | No       | `false` | Enables logging for the DNS Server event log. |
| `wincollect_agent_7_log_file_replication_service` | No | `false` | Enables logging for the File Replication Service event log. |
| `wincollect_agent_7_log_forwardedevents`   | No       | `false` | Enables logging for the Forwarded Events log. |
| `wincollect_agent_7_log_security`          | No       | `true`  | Enables logging for the Security event log. |
| `wincollect_agent_7_log_system`            | No       | `true`  | Enables logging for the System event log. |

### Event Filtering and Custom Queries

#### Filter Settings

Each event log source can be configured with specific filter settings using the following structure:

| Variable                                  | Required | Default | Description |
|-------------------------------------------|----------|---------|-------------|
| `wincollect_agent_7_filter_application` | No | | See description below. |
| `wincollect_agent_7_filter_directoryservice` | No | | See description below. |
| `wincollect_agent_7_filter_dns` | No | | See description below. |
| `wincollect_agent_7_filter_filereplicationservice` | No | | See description below. |
| `wincollect_agent_7_filter_forwardedevents` | No | | See description below. |
| `wincollect_agent_7_filter_security` | No | | See description below. |
| `wincollect_agent_7_filter_system` | No | | See description below. |

For each source, you must define two key variables: `type` and `events`.

- `type`: Determines the filter type and must be one of the following:
  - `nsalist`: Monitors a set of events recommended by the NSA.
  - `whitelist`: Only forwards events that are explicitly listed.
  - `blacklist`: Prevents the forwarding of listed events to IBM QRadar.
  - `nofilter`: No filtering is applied, and all events from the source are forwarded.
- `events`: A list of additional event IDs to be included or excluded, based on the `type` specified.

#### Example Filter Configuration

```yaml
wincollect_agent_7_filter_application:
  type: nsalist
  events:
    - 4624
    - 4625
```

#### Custom Queries

Custom queries provide a powerful mechanism for advanced event filtering based on specific criteria using XPath. This allows for precise control over which events are monitored and forwarded by the WinCollect agent.

| Variable                             | Required | Default | Description |
|--------------------------------------|----------|---------|-------------|
| `wincollect_agent_7_custom_queries`  | No       | -       | Defines a list of custom XPath queries for event filtering. Each query should specify both a `path` and a `query` to accurately target events. Limiting the number of queries is recommended to avoid potential performance issues. |

##### Query Structure

- `path`: Specifies the log path where the query should be applied, e.g., `Microsoft-Windows-Windows Firewall With Advanced Security/Firewall`.
- `query`: The XPath expression used to filter events within the specified path, e.g., `*[System[(Level=4 or Level=0) and (EventID=2004 or EventID=2005 or EventID=2006 or EventID=2009 or EventID=2033)]]`.

##### Example Custom Query Configuration

```yaml
wincollect_agent_7_custom_queries:
  - path: "Microsoft-Windows-Windows Firewall With Advanced Security/Firewall"
    query: "*[System[(Level=4 or Level=0) and (EventID=2004 or EventID=2005 or EventID=2006 or EventID=2009 or EventID=2033)]]"
```

Dependencies
------------

None.

Example Playbook
----------------

    - hosts: servers

      vars:
        wincollect_agent_7_installer_file: 'http://192.168.1.1:8080/pub/wincollect-7.3.0-24.x64.exe'
        wincollect_agent_7_target_address: 192.168.0.1
        wincollect_agent_7_target_protocol: udp
        wincollect_agent_7_target_port: 514
        wincollect_agent_7_log_security: true
        wincollect_agent_7_log_application: true
        wincollect_agent_7_log_system: true
        wincollect_agent_7_log_directory_service: false
        wincollect_agent_7_log_file_replication_service: false
        wincollect_agent_7_log_forwardedevents: false
        wincollect_agent_7_log_dns_server: false
        wincollect_agent_7_filter_security:
          type: nsalist
          events:
            - 4964
            - 4947
            - 4950
            - 4954
            - 4964
            - 5031
            - 5155
        wincollect_agent_7_filter_system:
          type: nsalist
          events:
            - 104
            - 1127
            - 5025
        wincollect_agent_7_filter_application:
          type: nsalist
          events:
            - 2
            - 104
        wincollect_agent_7_filter_dns:
          type: nofilter
          events: []
        wincollect_agent_7_filter_filereplicationservice:
          type: nofilter
          events: []
        wincollect_agent_7_filter_directoryservice:
          type: nofilter
          events: []
        wincollect_agent_7_filter_forwardedevents:
          type: nofilter
          events: []
        wincollect_agent_7_custom_queries:
          - path: "Microsoft-Windows-Windows Firewall With Advanced Security/Firewall"
            query: "*[System[(Level=4 or Level=0) and ( (EventID=2004 or EventID=2005 or EventID=2006 or EventID=2009 or EventID=2033) )]]"


      roles:
        - wincollect_agent_7

License
-------

MIT

Author Information
------------------

Mattias Jonsson
