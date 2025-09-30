# Ansible QUADS Self-Scheduler

This Ansible playbook automates the process of scheduling hosts on a QUADS server via its [self-scheduling REST API](https://github.com/redhat-performance/quads/blob/latest/docs/quads-self-schedule.md).

It handles user registration, login, host discovery, and scheduling, based on a simple configuration file and runtime parameters.

---
## Requirements

* **Ansible**: Ensure Ansible is installed on the machine where you run the playbook.
* **QUADS Server Access**: You need network access to the QUADS API server and valid user credentials.

---
## Configuration

Before running the playbook, you must edit the included `quads_config.yml` file in the same directory. This file stores your server and user details.

### Configuration Parameters

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
quads_password: "SomeSecurePassword123!"

# Host Selection Preferences
preferred_models:
  - "r650"
  - "r660"
  - "r640"
# preferred_models: "all"
```
### Running the Playbook:

## Scheduling a single host
```bash
ansible-playbook quads_self_schedule.yml -e "workload_name='My Test Workload'"
```

## Scheduling Three Hosts
```bash
ansible-playbook quads_self_schedule.yml -e "workload_name='My Test Workload'" -e "num_hosts='3'"
```

## Schedule a Host without Wiping the Disks
```
ansible-playbook quads_self_schedule.yml -e "workload_name='My Test Workload'" -e "wipe=false"
```
