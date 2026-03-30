# ageres210784/vmalert

An Ansible role which installs and configures [vmalert] on Linux

## Requirements

Ansible 2.8+

## Role Variables

You can see all vars in `defaults/main.yml` vars file.

If you use docker swarm, you should specify variables for the host on which
the container is supposed to be run, and also indicate the name of the swarm
manager through which the stack will be deployed in variable
`vm_swarm_manager`
```yaml
vm_swarm_manager: swarm-manager01
```

Built-in alert templates from `files/templates` can be controlled with include
and exclude lists:

```yaml
# Empty include list means "all bundled templates"
vmalert_templates_include: []

# Exclude from resulting set
vmalert_templates_exclude: []
```

Examples:

```yaml
# Deploy only selected bundled templates
vmalert_templates_include:
  - victoriametrics.yml
  - node_exporter.yml
  - postgres_exporter.yml

# Deploy all bundled templates except listed ones
vmalert_templates_exclude:
  - blackbox_exporter_ports.yml
  - blackbox_exporter_status.yml
```

When alert templates change, the role reloads vmalert differently based on
deployment mode:

- Docker container mode: sends `SIGHUP` to `{{ vmalert_container_name }}`.
- Docker swarm mode: forces update of `{{ vmalert_swarm_service_name }}`.

If your swarm service name differs from default, override:

```yaml
vmalert_swarm_service_name: my_stack_vmalert
```

## Dependencies

None

## Example Playbook

```yaml
- name: Ensure prometheus_alertmanager DB
  hosts: vmalerts
  remote_user: root

  roles:
    - ageres210784.vmalert
```

## License

Apache 2.0

## Author Information

This role was created by [Sergey Evseev].
