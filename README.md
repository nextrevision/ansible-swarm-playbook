# Ansible Swarm Playbook

Playbook for creating/managing a Docker Swarm cluster (requires Docker >= 1.12).

## Assumptions

This playbook assumes a running Docker daemon on the hosts and that the following Ansible inventory groups have been populated: `manager` and `worker`.

## Variables

You can override the `swarm_iface` variable in Ansible to determine the listening interface for your swarm hosts.

## `swarm.yml` vs. `swarm-facts.yml`

The `swarm.yml` playbook uses a shell statement for determining cluster membership where the `swarm-facts.yml` playbook uses the `docker_info_facts` module for injecting Docker info as facts then checking cluster membership against that.
