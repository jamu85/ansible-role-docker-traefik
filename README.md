# ansible-role-docker-traefik
This role configures and runs a simple [Traefik reverse proxy](https://traefik.io/) with Let's Encrypt, https redirect, password auth and source acl for the dashboard. As a feature you can enable the deployment of a [tutum/hello-world](https://hub.docker.com/r/tutum/hello-world/) container with Let's Encrypt behind traefik.

## Requirements

This role uses the [docker_compose](https://docs.ansible.com/ansible/latest/modules/docker_compose_module.html) module. Ensure to have all module requirements installed or, if you use Debian, take a look at my [ansible-docker-role](https://github.com/mambord/ansible-role-docker) to deploy docker on Debian Buster.

## Version
This playbook uses by default the latest traefik release. An image variable can be overwritten.

## Defaults
These are the default values for this playbook.

```yaml
---

#
# role features settings
#

# enable the hello-world container and set an appropriate domain
docker_traefik_example_webservice: true
docker_traefik_example_url: test.home.local

#
# traefik static configuration variables
#

# https://docs.traefik.io/observability/logs/#level
docker_traefik_log_level: ERROR

# https://docs.traefik.io/providers/docker/#exposedbydefault
docker_traefik_exposedByDefault: "false"

# email for lets encrypt
docker_traefik_le_email: admin@home.local

# https://docs.traefik.io/providers/docker/#network
docker_traefik_network: traefik

#
# dashboard settings
#

docker_traefik_dashboard_enable: "true"
docker_traefik_dashboard_url: dashboard.home.local

# default username / password: traefik / traefik
# look at https://docs.traefik.io/middlewares/basicauth/#users
docker_traefik_htpasswd: traefik:$apr1$MtQFeQ0G$XSlo.AMRzhsczSZOvZDI5.

#
# docker-compose settings
#

# traefik image:version
# see https://hub.docker.com/_/traefik
docker_traefik_image: traefik

# path to docker-compose and config files
docker_traefik_base_dir: /opt/traefik

# always pull image
docker_traefik_pull: yes

# stopped container
docker_traefik_stopped: no
```

## Source acl for dashboard
By defining the variable ``docker_traefik_dashboard_ip_whitelist`` you can enable source acl for the dashboard:
```
# whitelist single ip
docker_traefik_dashboard_ip_whitelist: "83.76.163.55"

#whitelist subnet
docker_traefik_dashboard_ip_whitelist: "192.168.1.0/24"
```
This role currently supports only one ip or subnet to be whitelisted.

## File tree
The role writes a file structure in the directory set by ``docker_traefik_base_dir`` as following:

```
/opt/traefik
    ├── data
    │   ├── acme.json
    │   ├── file-provider.yml
    │   └── traefik.toml
    ├── docker-compose.yml
    └── hello-world
        └── docker-compose.yml

```

## Donations
Thanks to the [Traefik Reddit](https://www.reddit.com/r/Traefik/) community for helping me out with problems.
