# CI: Deploy docker project Role

Deploy project in a docker container.

## Role Variables

- `project_tag` (default: latest): Tag name.
- `project_host` (default: "127.0.0.1"): Host for binding app port on docker server.
- `project_port` (default: 8080): Binding app port on docker server. Ignored if used `project_port_pool`.
- `project_app_port` (default: 8080): Exposed port in docker container.
- `project_port_pool` (optional): Ports pool.
- `project_name` (required): This name used in name of home folder path, hostname and in name of container.
- `project_hostname` (default: "{{ project_tag }}.{{ project_name }}.localhost")
- `nginx_index` (default: 50): Prefix in name of nginx config.
- `registry_username` (optional): Docker registry username.
- `registry_password` (optional): Docker registry password.
- `registry_hostname` (default: localhost): Docker registry address.
- `registry_container` (required): Image name in registry.
- `docker_project_image` (default: "{{ registry_hostname }}/{{ registry_container }}:{{ project_tag }}")
- `docker_project_name` (default: "{{ project_name }}-{{ project_tag }}")
- `docker_project_home` (default: "/opt/{{ docker_project_name }}")
- `docker_project_env` (optional)
- `docker_project_volumes` (optional)
- `docker_project_ports` (optional): Additional applications ports.
- `docker_project_network_mode` (default: host): Connect the container to a network.
- `docker_project_networks` (default: []): List of networks the container belongs to.
- `docker_project_log_driver` (default: json-file): Specify the logging driver.
- `docker_project_log_options` (optional): Dictionary of options specific to the chosen log_driver. See [Configure logging drivers](https://docs.docker.com/engine/admin/logging/overview/) for details.
- `docker_project_remove_existing_container`: yes

## Dependencies

- `docker` or `docker-ce`
- `nginx`

## Example Playbook

```yaml
- hosts: all
  become: yes
  become_method: sudo
  vars:
    project_tag: PR-21
    project_name: my_app
    project_app_port: 5000
    project_port_pool: 5800-5899
    project_hostname: "{{ project_tag }}.{{ project_name }}.myhost.com"
    nginx_index: 35
    registry_hostname: registry.myhost.com
    registry_container: my_org/my_app
    registry_username: ci-bot
    registry_password: secret
  roles:
    - role: levonet.docker_project_deploy
```

## License

[MIT](https://opensource.org/licenses/MIT)

## Author Information

This role was created by [Pavlo Bashynskyi](https://github.com/levonet)
