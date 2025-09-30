# Ansible QUADS Self-Scheduler

[![GHA](https://github.com/quadsproject/ansible-quads-ssm/actions/workflows/ansible-lint.yml/badge.svg)](https://github.com/quadsproject/ansible-quads-ssm/actions)

Fully automates the process of scheduling hosts on a QUADS server via its [self-scheduling REST API](https://github.com/redhat-performance/quads/blob/latest/docs/quads-self-schedule.md).

## Features

* Performs all self-service steps:  registration, login, host discovery and scheduling
* Allows scheduling multiple hosts or passing additional cloud values e.g. `qinq: 1` or `nowipe`
* Allows setting a host model preference order, or use `all` if you don't care
* Generates your list of schedule hosts and login/authentication details locally

---
## Requirements

* **Ansible**: Ensure Ansible is installed on the machine where you run the playbook.
* **QUADS Server Access**: You need network access to the QUADS API server and valid user credentials.

---
## Configuration

> [!IMPORTANT]
> You must configure `quads_config.yml` first for at least `quads_api_server`, `quads_username`, and `quads_password`

### Configuration Parameters

> [!NOTE]
> If it's your first time talking to a new QUADS server simply make up a password in `quads_config.yml` and use that going forward for that server.

| Parameter          | Type          | Description                                                                                                                                                               |
| ------------------ | ------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `quads_api_server` | String        | The FQDN or IP address of your QUADS API server (e.g., `"quads.example.com"`).                                                                                             |
| `quads_username`   | String        | Your username, which is the part of your email before the `@` sign (e.g., `"joe"`).                                                                                       |
| `quads_user_domain`| String        | The domain part of your email address (e.g., `"example.com"`).                                                                                                              |
| `quads_password`   | String        | Your password for the QUADS server.                                                                                                                                       |
| `preferred_models` | List or String| A list of server models to schedule, in order of preference. The playbook will try to find a match for the first model, then the second, and so on. Use `"all"` to select any available model without preference. |

### Example `quads_config.yml`

```yaml
---
# QUADS Server Configuration
quads_api_server: "quads.example.com"

# User Details
quads_username: "joe"
quads_user_domain: "example.com"
quads_password: "make_a_password_up"

# Host Selection Preferences
preferred_models:
  - "r650"
  - "r660"
  - "r640"
# preferred_models: "all"
```
## Running the Playbook:

> [!NOTE]
> You must wrap `workload_name` description in single quotes like the below examples.

### Scheduling a single host
```bash
ansible-playbook quads_self_schedule.yml -e "workload_name='My Test Workload'"
```

### Scheduling Three Hosts
```bash
ansible-playbook quads_self_schedule.yml -e "workload_name='My Test Workload'" -e "num_hosts='3'"
```

### Schedule a Host without Wiping the Disks
```bash
ansible-playbook quads_self_schedule.yml -e "workload_name='My Test Workload'" -e "wipe=false"
```

## Schedule a Host with QINQ 1 VLAN Mode
```bash
ansible-playbook quads_self_schedule.yml -e "workload_name='My Test Workload'" -e "qinq='1'"
```
