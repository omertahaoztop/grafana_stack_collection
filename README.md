# Ansible Collection - fahcsim.grafana_stack
[![CI](https://github.com/fahcsim/grafana_stack_collection/actions/workflows/prometheus.yml/badge.svg)](https://github.com/fahcsim/grafana_stack_collection/actions/workflows/prometheus.yml)  
[![Release](https://github.com/fahcsim/grafana_stack_collection/actions/workflows/release.yml/badge.svg)](https://github.com/fahcsim/grafana_stack_collection/actions/workflows/release.yml)
### Version 1.1.1
This should be enough to set up centralized logging and monitoring fairly easily. Prometheus will be configured to monitor every host in the inventory (see roles/prometheus/templates/prometheus.yml.j2). Loki agents will point to the loki server(configured as `loki_server` variable). All of these services are installed as binaries with systemd.
### This collection has a role for each of the following:
- Prometheus
- Loki
- Prometheus Node Exporter
- Promtail
## Requirements
For the prometheus config to work correctly, the ansible inventory hostname of each server must be a DNS name that your prometheus server can resolve.
Current supported distributions are Debian 10/11 and Ubuntu 20/22. It may work on other distros, but it has not been tested.
## Example Playbook
```
- name: bootstrap monitoring server
  hosts: monitoring
  become: True
  vars:
    loki_server: loki_server.local
  roles:
    - fahcsim.grafana_stack.prometheus
    - fahcsim.grafana_stack.loki
    - fahcsim.grafana_stack.prometheus_node_exporter
    - fahcsim.grafana_stack.promtail
    - fahcsim.grafana_stack.grafana
```
### Updating
Each service can be updated by changing the `<service>_version` variable, as long as a corresponding version exists in the project's github repo.
#### Current Defaults
##### Prometheus
`prometheus_version: 2.37.0`
##### Loki
`loki_version: 2.6.1`
##### Prometheus Node Exporter
`node_exporter_version: 1.3.1`
##### Promtail
`promtail_version: 2.6.1`
`loki_server: localhost` - change this to whatever host you want to use as the loki server

### Node Exporter Offline Installation
set the variable `node_exporter_binary_local_dir` to the path of the node exporter binary you copied locally